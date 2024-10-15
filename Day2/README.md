# Day 2

## Info - Container Orchestration Platform Overview
<pre>
- Container Orchestration Platforms supports the below features  
  - deploying containerized application workloads
  - scale up/down based on user-traffic to our applications
  - provides an environment to make our applications highly available(HA)
  - load-balancing
  - monitoring tools
  - rolling update
    - upgrading your live application from one version to the other without any downtime
  - rollback
    - rolling back to previous to older versions when the new version is found to be unstable
  - exposing application for internal or external use via services
  - supports service discovery - i.e services can be accessed using their name 
- examples
  - Docker SWARM
  - Google Kubernetes
  - Opensource Openshift (OKD)
  - Red Hat Openshift
  - EKS - AWS Managed Kubernetes Service 
  - AKS - Azure Kubernetes Service
  - ROSA - AWS Managed Openshift Service
  - ARO - Azure Managed Openshift Service
</pre>


## Info - Kubernetes Overview

## Info - Kubernetes High Level Architecture


## Setup 3 node Kubernetes cluster using Minikube
```
docker --version
docker images
minikube config set driver docker
minikube config set rootless true
minikube start --memory='32g' --cpus='8' -n 3
```
