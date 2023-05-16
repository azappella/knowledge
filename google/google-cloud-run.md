# google cloud run

## Add .gcloudignore

Always add a .gcloudignore (even if empty) to make sure the .gitignore doesn't remove some important build files from the container

## Build

```
docker build . --tag IMAGE_URL
```

- https://cloud.google.com/run/docs/building/containers

## Deploy

```
gcloud run deploy SERVICE --source .
```

Where SERVICE is the name of your service

- https://cloud.google.com/run/docs/deploying-source-code

## env vars

```
--set-env-vars
--update-env-vars
--remove-env-vars
--clear-env-vars
```

```
gcloud run deploy [SERVICE] --image IMAGE_URL --update-env-vars KEY1=VALUE1,KEY2=VALUE2
```

or

```
gcloud run services update SERVICE --update-env-vars KEY1=VALUE1,KEY2=VALUE2
```

## References

- https://cloud.google.com/run/docs/fit-for-run
- https://cloud.google.com/run/docs/setup
- https://cloud.google.com/run/docs/container-contract
- https://cloud.google.com/run/docs/about-execution-environments
- https://github.com/GoogleCloudPlatform/cloud-run-microservice-template-go
- https://cloud.google.com/run/docs/authenticating/public#gcloud
