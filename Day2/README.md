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


## Info - Kubernetes Control Plane
<pre>
- Control Plane Components only run in master nodes
- below are the Control Plane Components
  1. API Server
  2. etcd database
  3. scheduler
  4. Controller Managers
</pre>

#### Info - API Server
<pre>
- is the heart of Kubernetes - Container Orchestration Platform  
- it supports REST API for all Kubernetes features
- API Server is the only components that has write access to etcd database
- API Server saves and manages the cluster and application status in the etcd database
- every API Server has its own dedicated etcd database
- Whenever new records are added, edited, deleted in the etcd database, API Server will send broadcasting events about the change
</pre>

#### Info - etcd
<pre>
- it is key/value database
- it is an independent opensource database that is developed and maintained independent of Kubernetes/Openshift
- distributed database that can be used even outside the scope of Kubernetes/Openshift
</pre>

#### Info - scheduler
<pre>
- this component is responsible to schedule containerized application loads onto healthy nodes within the Kubernetes cluster
- when we deploy our application into Kubernetes(k8s), it creates Pods, within Pods containers are created, our application runs inside one of those containers
- the scheduler decides on which Pod goes into which node ( master-1, master-2, master-3, worker-1, worker-2, worker-3 )
- however, it won't schedule the Pods on its own, it is send the scheduling recommendations to API Server via REST calls
</pre>

#### Info - Controller Managers
<pre>
- Controller Managers is combination of many Controllers
- monitoring feature and self-healing property are supported by Controllers to our containerized applications
- For example
  - Deployment Controller
  - ReplicaSet Controller
  - DaemonSet Controller
  - Endpoint Controller
  - Storage Controller
  - Job Controller
  - CronJob Controller
- Controller is special type of application that has unrestricted access to all namespaces(projects) within Kubernetes
- Each Controller monitors one type of Kubernetes Resource
- For example
  - Deployment Controller Manages ReplicaSet
  - ReplicaSet Controller Manages Pods
</pre>

## Pod Overview
<pre>
- literal english - a group of whales is called a Pod
- it is a JSON file that is stored in etcd database
- docker logo is whale, that's how the Pod terminology was coined
- is a group of related containers
- a Kubernetes resource which is defined as JSON document
- Pod definition is stored in etcd and maintained by API Server
</pre>


## ReplicaSet Overview
<pre>
- is a Kubernetes resource, that resides in etcd database
- it is a JSON file that is stored in etcd database
- it captures the below details
  - container image that must be used while deploying the application containers
  - desired number of Pod instances that must be running
  - actual number of Pod instances that are running
- ReplicaSet controller takes the ReplicaSet definition as an input and it ensures the desired and actual Pod counts are matching always
</pre>

## Deployment Overview
<pre>
- is a Kubernetes resource, that resides in etcd database
- it is a JSON file that is stored in etcd database
- it captures the below details
  - name of the deployment
  - container image that must be used while deploying the respective Pods
  - number of Pod instances that must be running
- Deployment Controller takes the Deployment definition as an input and it ensures the desired and actual Pod counts are matching
  - Deployment Controller creates the ReplicaSet with desired number of Pod instance count, while the ReplicaSet controller is the one that manages the Pods for the respective ReplicaSet
- is used to deploy stateless application
</pre>

## Setup 3 node Kubernetes cluster using Minikube
```
docker --version
docker images
minikube delete
minikube start --memory='32g' --cpus='8' -n 3 --driver=virtualbox
```
