# Day 2

## Setup 3 node Kubernetes cluster using Minikube
```
docker --version
docker images
minikube config set driver docker
minikube config set rootless true
minikube start --memory='32g' --cpus='8' -n 3
```
