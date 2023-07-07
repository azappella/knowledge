# make & Makefile

## Makefile template for tagging and pushing docker image

```
NAME   := docker/image
TAG    := $$(git log -1 --pretty=%h)
IMG    := ${NAME}:${TAG}
IMG_LATEST := ${NAME}:latest

# GCP
# REGISTRY=eu.gcr.io
# PROJECT_ID=atlas-app-206110

all: docker-build tag tag-registry

publish: all push

docker-build:
	docker build -t ${NAME} .

tag:
	docker tag ${NAME} ${IMG}

tag-registry:
	docker tag ${NAME} ${REGISTRY}/${PROJECT_ID}/${NAME}

push:
	docker push ${REGISTRY}/${PROJECT_ID}/${NAME}
```

## Docker example

```
MAJOR?=0
MINOR?=1

VERSION=$(MAJOR).$(MINOR)

APP_NAME = "system-toolbox"

# Our docker Hub account name
# HUB_NAMESPACE = "<hub_name>"

# location of Dockerfiles
DOCKER_FILE_DIR = "dockerfiles"
DOCKERFILE = "${DOCKER_FILE_DIR}/Dockerfile"

IMAGE_NAME = "${APP_NAME}"
CUR_DIR = $(shell echo "${PWD}")

# For python format checker. Default is 78
PEP8_MAX_LINE_LENGTH = 99

# unit testing options
NOSETEST_OPTS = --verbosity=--include='.*_test.py' --detailed-errors --where=toolbox
COVERAGE_OPTS = --with-coverage --cover-package=toolbox --cover-html --cover-inclusive --cover-tests --cover-erase \
--cover-html-dir=../test-results/unit-test-code-coverage

#################################
# Install targets
#################################
.PHONY: install-dev
install-dev:
 @echo "+ $@"
33
 @pip install -e .

.PHONY: pip-freeze
 @echo "+ $@"
 @pip freeze | grep -v system_toolbox > pip-requirements/requirements.txt


#################################
# Docker targets
#################################
.PHONY: clean-image
clean-image: version-check
 @echo "+ $@"
 @docker rmi ${HUB_NAMESPACE}/${IMAGE_NAME}:latest  || true
 @docker rmi ${HUB_NAMESPACE}/${IMAGE_NAME}:${VERSION}  || true

.PHONY: image
image: version-check
 @echo "+ $@"
 @docker build -t ${HUB_NAMESPACE}/${IMAGE_NAME}:${VERSION} -f ./${DOCKERFILE} .
 @docker tag ${HUB_NAMESPACE}/${IMAGE_NAME}:${VERSION} ${HUB_NAMESPACE}/${IMAGE_NAME}:latest
 @echo 'Done.'
 @docker images --format '{{.Repository}}:{{.Tag}}\t\t Built: {{.CreatedSince}}\t\tSize: {{.Size}}' | \
       grep ${IMAGE_NAME}:${VERSION}

.PHONY: push
push: clean-image image
 @echo "+ $@"
 @docker push ${HUB_NAMESPACE}/${IMAGE_NAME}:${VERSION}
 @docker push ${HUB_NAMESPACE}/${IMAGE_NAME}:latest

#################################
# test targets
#################################
.PHONY: test-unit
test-unit:
 @echo "+ $@"
 nosetests ${NOSETEST_OPTS}

.PHONY: check-fmt
#check-fmt: image
check-fmt:
 @echo "+ $@"
 pycodestyle --filename='*.py' --exclude='*.sh,*.md,*.txt,Makefile,*.swp' --max-line-length=${PEP8_MAX_LINE_LENGTH} *

.PHONY: test-static
test-static:
 @echo "+ $@"
 pylint -d duplicate-code test-cases
pylint  toolbox
 pylint integration-tests

.PHONY: test-all
test-all: check-fmt test-static test-unit

.PHONY: test-container
test-container: image
 @echo "+ $@"
 @docker run --rm --name toolbox-unit-tests ${HUB_NAMESPACE}/${IMAGE_NAME}:latest make test-all
 @docker run --rm --name toolbox-int --volume ${CUR_DIR}/results:/root/logs -e REGISTRY_USERNAME=foo -e REGISTRY_PASSWORD=bar \
     ${HUB_NAMESPACE}/${IMAGE_NAME}:latest python ./integration-tests/testbed_validation.py

.PHONY: integration-static
integration-static: image
 @echo "+ $@"
 @docker run --rm --name toolbox-int --volume ${CUR_DIR}/results:/root/logs -e REGISTRY_USERNAME=foo -e REGISTRY_PASSWORD=bar \
     ${HUB_NAMESPACE}/${IMAGE_NAME}:latest python ./integration-tests/testbed_validation.py
    #@python ./integration-tests/testbed_validation.py

.PHONY: integration-testbed-survey
integration-testbed-survey:
    @echo "+ $@"
    @python ./integration-tests/testbed_validation.py --topology-filter=poc_ --sut-filter=${TESTBED_SURVEY_SUT} \
    --create-system ${INTEGRATION_PERSONA} ${DRY_RUN}


#################################
# Utilities
#################################

.PHONY: version-check
version-check:
    @echo "+ $@"
    if [ -z "${VERSION}" ]; then \
      echo "VERSION is not set" ; \
      false ; \
    else \
      echo "VERSION is ${VERSION}"; \
    fi
```


## Another Docker example
```
# DOCKER_REPO is the fully-qualified Docker repository name.
ifndef DOCKER_REPO
$(error "DOCKER_REPO must be defined in the project's Makefile.")
endif

# DOCKER_TAGS is a space-separated list of tag names used when building a Docker
# image. The list defaults to just 'dev'. Note that the 'dev' tag cannot be
# pushed to the registry.
DOCKER_TAGS ?= dev

# DOCKER_BUILD_REQ is a space separated list of prerequisites needed to build
# the Docker image.
DOCKER_BUILD_REQ +=

# DOCKER_BUILD_ARGS is a space separate list of additional arguments to pass to
# the "docker build" command.
DOCKER_BUILD_ARGS +=

# DOCKER_PLATFORMS is a list of the target platforms for the Docker image.
DOCKER_PLATFORMS ?= linux/amd64

################################################################################

# _DOCKER_PUSH_GUARDS is a list of phony targets that are used as prerequisites
# to the docker-push target to prevent pushing "dev" tags.
_DOCKER_PUSH_GUARDS := $(foreach TAG,$(DOCKER_TAGS),_docker-push-guard-$(TAG))

# _DOCKER_BUILD_REQ is the union of DOCKER_BUILD_REQ and any other files that
# are always considered prerequisites to Docker builds.
_DOCKER_BUILD_REQ = Dockerfile .dockerignore $(DOCKER_BUILD_REQ)

# _DOCKER_TAG_FLAGS is the set of --tag flags to pass to the build command.
_DOCKER_TAG_FLAGS = $(foreach TAG,$(DOCKER_TAGS),--tag "$(DOCKER_REPO):$(TAG)")

# _DOCKER_PLATFORM_FLAG is the --platform flag to pass to the build command.
_DOCKER_PLATFORM_FLAG = --platform $(shell echo $(DOCKER_PLATFORMS) | tr ' ' ',')

################################################################################

# docker --- Builds a docker image for the current platform and "loads" the
# image into the local Docker server.
.PHONY: docker
docker: $(_DOCKER_BUILD_REQ)
	docker buildx build \
		$(_DOCKER_TAG_FLAGS) \
		$(DOCKER_BUILD_ARGS) --build-arg "VERSION=$(SEMVER)" \
		--pull \
		--load \
		.

# docker-test --- Builds docker images for each target platform then
# discards the result.
.PHONY: docker
docker-test: $(_DOCKER_BUILD_REQ)
	docker buildx build \
		$(_DOCKER_TAG_FLAGS) \
		$(_DOCKER_PLATFORM_FLAG) \
		$(DOCKER_BUILD_ARGS) --build-arg "VERSION=$(SEMVER)" \
		--pull \
		.

# docker-build --- Builds docker images for each target platform and pushes
# those images, and a manifest list to the registry.
.PHONY: docker-push
docker-push: $(_DOCKER_BUILD_REQ) $(_DOCKER_PUSH_GUARDS)
	docker buildx build \
		$(_DOCKER_TAG_FLAGS) \
		$(_DOCKER_PLATFORM_FLAG) \
		$(DOCKER_BUILD_ARGS) --build-arg "VERSION=$(SEMVER)" \
		--pull \
		--push \
		.

################################################################################

# Treat any dependencies of the Docker build as secondary build targets so that
# they are not deleted after a successful build.
.SECONDARY: $(DOCKER_BUILD_REQ)

.dockerignore:
	@echo .makefiles > "$@"
	@echo .git >> "$@"
	@echo .github >> "$@"

.PHONY: $(_DOCKER_PUSH_GUARDS)
_docker-push-guard-dev:
	@echo "The 'dev' tag can not be pushed to the registry, did you forget to set the DOCKER_TAGS environment variable?"
	@exit 1

_docker-push-guard-%:
```