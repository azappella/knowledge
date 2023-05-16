# gcloud

## installation

Extract the archive, then
```
./google-cloud-sdk/install.sh
```

Initialize
```
./google-cloud-sdk/bin/gcloud init
```

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

## create config for project

```
gcloud init
```

## switch between project configurations

```
gcloud config configurations list
gcloud config configurations activate [name]
```

## list accounts whose auth are stored on the system

```
gcloud auth list
```

## List properties in your active gcloud CLI config

```
gcloud config list
```

## View information about your gcloud CLI installation and the active configuration

```
gcloud info
```

## install gcloud components

```
gcloud components install COMPONENT_ID
```

Usefuls components
```
gcloud components install app-engine-python
gcloud components install app-engine-python-extras
gcloud components install cbt
gcloud components install bigtable
gcloud components install terraform-tools
```