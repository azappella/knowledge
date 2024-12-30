# docker compose

## Install manually

To download and install the Docker Compose CLI plugin, run:

```
 DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}

 mkdir -p $DOCKER_CONFIG/cli-plugins

 curl -SL https://github.com/docker/compose/releases/download/v2.32.1/docker-compose-darwin-aarch64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
```
This command downloads and installs the latest release of Docker Compose for the active user under $HOME directory.

To install:

Docker Compose for all users on your system, replace ~/.docker/cli-plugins with /usr/local/lib/docker/cli-plugins.
A different version of Compose, substitute v2.32.0 with the version of Compose you want to use.
    For a different architecture, substitute x86_64 with the architecture you want

    .

Apply executable permissions to the binary:

```
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
```

or, if you chose to install Compose for all users:

```
 sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```
Test the installation.

```
 docker compose version
```

Expected output:

Docker Compose version v2.32.0

Source: https://docs.docker.com/compose/install/linux/

https://github.com/docker/compose/releases/download/v2.32.1/docker-compose-darwin-aarch64