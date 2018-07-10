# gcloud

## change project

```
gcloud auth login
gcloud config set project "project-name"
gcloud config set compute/zone "europe-west3-b"
gcloud config set compute/region "europe-west3"
```

## connect to compute engine with ssh using gcloud

```
gcloud compute --project "project-name" ssh --zone "europe-west3-b" "project-name"
```