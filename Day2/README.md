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

## Info - Docker SWARM
<pre>
- Docker SWARM is a Container Orchestration Platform developed by Docker Inc as a open source project
- it is Docker's native Container Orchestration Platform
- it only support Docker containerized application workloads
- it is very light-weight, easy to setup, easy to learn and easy to maintain
- it is not production grade
- it is very good for learning or dev/qa environment or for R&D purpose
</pre>

## Info - Kubernetes Overview
<pre>
- Container Orchestration Platform developed by Google 
- Google used this product internally for several years before they made it opensource
- it is time-tested, robus Container Orchestration Platform thats works for simple or very complex applications
- ideal for dev/qa/prod 
- supports many different container runtimes and engines 
- it is an opensource project that can be used free of cost for personal and commercial use
- supports only command-line, setup can only be done in Linux
- we don't get support from Google as it is opensource project
- however, we can get support from Google if we use GKE from Google cloud
- however, we can get support from Amazon if we use EKS from AWS cloud
- however, we can get support from Microsoft if we use AKS from Azure cloud  
</pre>

## Info - OKD
<pre>
- opensource OpenShift Container Orchestration Platform
- supports CRI-O container runtime and Podman Container Engine only
- supports CLI and webconsole
- supports user management
- developed on top of Kubernetes with many addition features
- it is a superset of Google Kubernetes
- you won't get support from any specific company as it is open source, you are on your own, you only get community support
</pre>

## Info - Red Hat Openshift
<pre>
- it is commercial Container Orchestration Platform developed by Red Hat
- developed on top of opensource OKD
- supports CLI and Webconsole
- we will get support from Red Hat ( an IBM company )
</pre>

## Info - Kubernetes High Level Architecture


## Setup 3 node Kubernetes cluster using Minikube
```
docker --version
docker images
minikube config set driver docker
minikube config set rootless true
minikube start --memory='32g' --cpus='8' -n 3
```
