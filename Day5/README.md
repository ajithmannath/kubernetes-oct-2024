# Day5

## Lab connection details

<pre>
From the RPS cloud windows machine you need to open Remote Desktop connection and type
  
Server 1 ( 10.0.1.13 )
- user01 to user08
- user17 to user20

Server 3 ( 10.0.1.25 )
- user09 to user16
- user21 to user24
</pre>  


## Red Hat Openshift Overview
<pre>
- is Red Hat's Kubernetes distribution
- developed on top of opensource Google Kubernetes with many additional features
- it is a superset of Kubernetes, hence all features of Kubernetes are also supported in Openshift
- Red Hat Openshift supports only RHCOS in master nodes and either RHCOS/RHEL in worker nodes
- supports only CRI-O Container runtime and Podman Container engine
- enterprise product that requires commercial license
- supports many additional features
  - Web console
  - Internal Openshift Image Registry
  - Source to Image (S2I)
    - deploying application from source code
    - deploying application using Dockerfile
  - supports CI/CD
  - supports routes to expose application for external access
  - supports user management
- AWS supports managed Red Hat Openshift cluster called ROSA
  - Load Balancer creates an external Load Balancer supported by AWS
  
- Azure supports managed Red Hat Openshift cluster called ARO
  - Load Balancer creates an external Load Balancer supported by Azure

- As Red Hat Openshift makes use of Red Hat Enterprise Core OS, it is secure already
  - Ports below 1024 are not allowed as it is reserved for internal use
  - not all applications can be deployed with root access
  - it can be made more secure by using network policy like we do in Kubernetes
  - it will enforce best practices are followed which are not taken so seriously
</pre>

## Info - Openshift onPrem vs Cloud
<pre>
- onPrem Openshift 
  - installation of openshift we need to take care
  - Red Hat Openshift license are taken care by us
  - backup is our responsibility
  - we need to decide the master/worker node hardware configuration as per our application workload and user traffic
  - add new nodes into cluster is done manually using Openstack, VMWare vSphere, etc.,
  - Metallb operator or similar operators must be used to support LoadBalancer service in a on-prem openshift setup like our lab setup

- AWS ROSA
  - installation of openshift is taken care by AWS
  - Red Hat Openshift license is taken care by AWS
  - Hardware configuration of master nodes are decided and managed by AWS
  - backup of etcd database is taken care by AWS
  - LoadBalancer service when created it will automatically create AWS ALB/ELB as it is tighly integrated with AWS
</pre>

## Red Hat Openshift High-Level Architecture
![openshift](openshiftArchitecture.png)
![openshift](master-node.png)

## Kube config file
- the oc/kubectl client tools requires a config file that has connection details to the API Server(load balancer)
- the config file is generally kept in user home directory, .kube folder and the default name of kubeconfig is config
- optionally we could also use the --kubeconfig flag with the oc command to point to a config file
- it is also possible to use a KUBECONFIG environment variable to point to the config file
- Just to give an idea, it is possible that your Kubernetes/OpenShift is running in AWS/Azure but you could install oc/kubectl client tool on your laptop with a config file and still run all the oc/kubectl commands from your laptop without going to aws/azure

To print the content of kubeconfig file
```
cat ~/.kube/config
```
## About Red Hat Enterpise Core OS ( RHCOS )
- an optimized operating system created especially for the use of Container Orchestration Platforms
- each version of RHCOS comes with a specific version of Podman Container Engine and CRI-O Container Runtime
- RHCOS enforces many best practices and security features
- it allows writing to only folders the application will has read/write access
- if an application attempts to modify a read-only folder RHCOS will not allow those applications to continue running
- RHCOS also reserves many Ports for the internal use of Openshift
- User applications will not have write access to certain reserved folders, user applications are allowed to perform things as non-admin users only, only certain special applications will have admin/root access

### Points to remember
- Red Hat Openshift uses RedHat Enterprise Linux Core OS
- RHCOS has many restrictions or insists best practises
- RHEL Core OS reserves ports under 1024 for its internal use
- Many folders within the OS is made as ready only
- Any application Pod attempts to perform write operation on those restricted folders will not be allowed to run
- For detailed documentation, please refer official documentation here https://docs.openshift.com/container-platform/4.8/architecture/architecture-rhcos.html

## Info - Pod Lifecycle
- Pending - Container image gets downloads or there are no Persistent Volume to bound and claim them
- Running - The Pod is scheduled to a node and all containers in the Pod are up and running
- Succeeded - All containers in the Pod have terminated succesfully and not be restarted
- Failed - All containers in the Pod have terminated but one or more containers terminated with non-zero status or was terminated by Openshift
- Unknown - For some reason, the state of the Pod could not be obtained may be there is some problem in communicating to the node where the Pod is running

## Info - Container Lifecycle
- Waiting - pulling the container image
- Running - container is running without issues
- Terminated - container in the Terminated state began execution and then either ran to completion or failed for some reason

## Info - NodePort vs Route
<pre>
- NodePort is an external service
- It is a K8s features, which is also supported in openshift
- Kubernetes/Openshift reserve ports in range 30000-32767 for the purpose of NodePorts
- For each, NodePort service we create one of the ports from the above range will be alloted for the service
- the chose nodeport is opened in all the nodes for the nodeport service
- if we create 100 nodeport services, we end up opening 100 firewall ports on all the nodes, which is a security concern
- also nodeport service is not end-user friendly or developer friendly as they are accessed via node hostname/ip address, ideally the end-user should not have worry about about how many nodes are part of openshift
- routes is based on Kubernetes ingress, which provides an easy to access public url which is user-friendly as opposed to nodeport service
- hence, in openshift for internal service, we can create clusterip service
- for external access, we just need to expose the clusterip service as a route
- we don't have use node-port service in openshift
</pre>


## Info - Deployment vs DeploymentConfigs
<pre>
- In older version of Kubernetes, we had to use ReplicationController to deploy applications into Kubernetes/Openshift
- The Red Hat openshift team, at that time added DeploymentConfig to allow deploying application in the declarative style as the ReplicationController doesn't support deploying application in the declarative style
- Meanwhile, the Google Kubernetes team & community added Deployment and ReplicaSet resource as an alternate for ReplicationController
- Hence, in Openshift the Red Hat team deprecated the use of DeploymentConfig as Deployment and DeploymentConfig pretty does the same
- Kubernetes, deprecated the use of ReplicationController
- Hence, whenever we deploy new appplication we need choose Deployment over the DeploymentConfig as DeploymentConfig internally uses ReplicationController
</pre>

## Lab - List the openshift nodes
```
oc get nodes
kubectl get nodes
oc get nodes -o wide
kubectl get nodes -o wide
oc version
kubectl version
```

Expected output
![image](https://github.com/user-attachments/assets/958721c9-6663-468e-901e-5e80e0edd711)
![image](https://github.com/user-attachments/assets/1e371900-5d33-4f2d-a24e-9c0f67419009)


## Lab - Finding more details about an openshift node
```
oc describe node/master01.ocp4.rps.com
```

Expected output
![image](https://github.com/user-attachments/assets/94b4d493-8247-42ba-81bc-5f260fa8dc7c)
![image](https://github.com/user-attachments/assets/d82dcdd8-d4d8-4926-b4f3-3ed21e3efa28)

## Lab - Create a new project

The below command will create a new project and switches to the project
```
oc new-project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/91353271-7aca-46de-b97c-e33cee4d6ea3)

## Lab - Let's create a nginx deployment 
```
oc project jegan
oc create deploy nginx --image=nginx:latest --replicas=3
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/6d3acf2b-cd26-4c39-a01b-7a3386a5f39d)

## Lab - Deleting a project along with all resources in it
```
oc project
oc get all
oc delete project jegan
oc get projects | grep jegan
```

Expected output
![image](https://github.com/user-attachments/assets/7612b691-92bd-415c-84eb-3db2663b908b)

## Lab - Deploying an application in openshift S2I(source to Image) 
```
oc project jegan
oc new-app --name=hello https://github.com/tektutor/spring-ms.git
```

Expected output
![image](https://github.com/user-attachments/assets/e68ac0bb-21b9-40b1-b99e-6fc12e7eb3ff)
![image](https://github.com/user-attachments/assets/9724cbd9-b117-431c-b1c2-4f4273156f0e)

## Lab - Importing image into Openshift Internal registry
```
oc import-image ubi8/openjdk-11:1.20-2.1727147549 --from=registry.access.redhat.com/ubi8/openjdk-11:1.20-2.1727147549 --confirm
```
Expected output
![image](https://github.com/user-attachments/assets/1b941ed4-c9fb-4f25-911f-003c46c29dab)
![image](https://github.com/user-attachments/assets/3ecd6f7c-4596-4b80-9776-fa987c7150bd)
![image](https://github.com/user-attachments/assets/805b0b2a-65dc-4b4d-9403-f15861cfea2d)
![image](https://github.com/user-attachments/assets/f770b2d8-7d39-4de2-866c-919a731750d1)


## Lab - Deploy your custom application using S2I source strategy
```
oc new-project jegan
oc new-app --name=hello openjdk-11:1.20-2.1727147549~https://github.com/tektutor/spring-ms.git --strategy=source
```
Expected output
![image](https://github.com/user-attachments/assets/82277dda-d978-4221-815e-3ba4a571a4c7)
![image](https://github.com/user-attachments/assets/cfa0d14a-a1f8-4af6-aee6-8e823e8b7d5d)
![image](https://github.com/user-attachments/assets/04496dca-519d-4b29-ba5b-b3e13f77cbe8)
![image](https://github.com/user-attachments/assets/458a10f5-d96e-4378-8bdf-9489ff938004)
![image](https://github.com/user-attachments/assets/cbe3c029-5cf7-4f83-a4d3-3fd8fc46acbc)
![image](https://github.com/user-attachments/assets/d86e3322-a1c5-4363-9421-6308e1bf38a7)
![image](https://github.com/user-attachments/assets/21e891ec-61d9-4cb9-91fb-3de70093f0f4)
![image](https://github.com/user-attachments/assets/49e12846-fff5-44f6-902f-719979b765b3)

## Lab - Creating a build declaratively using a custom Buildconfig via yaml file
```
cd ~/kubernetes-oct-2024
git pull
cd Day5/Buildconfig
oc create is tektutor-spring-hello
oc get imagestreams
oc get imagestream
oc get is
oc describe is/tektutor-spring-hello

oc apply -f buildconfig.yml
```

Expected outupt
![image](https://github.com/user-attachments/assets/00301d75-0c59-4ac7-ba24-52c7d6089543)
![image](https://github.com/user-attachments/assets/5f051a4b-5a60-4d5a-8a9a-efb5de32b572)
![image](https://github.com/user-attachments/assets/f3adbaae-1514-492e-b992-7f0e10a0893e)
![image](https://github.com/user-attachments/assets/0fb82eee-2cba-4859-922b-ef4bbcc9d00f)
![image](https://github.com/user-attachments/assets/c671e5ca-8ebc-4b35-b38c-83ab788df00f)
![image](https://github.com/user-attachments/assets/92cdc360-32b9-4805-9685-37c8d6d9cbed)
![image](https://github.com/user-attachments/assets/5aac37ff-0515-4886-95c8-645b3e1c5c48)


## Further references
<pre>
https://kubernetes.io/docs/concepts/cluster-administration/networking/#the-kubernetes-network-model

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-networking

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-network-policy

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-kubernetes-ingress

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-kubernetes-egress

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-kubernetes-services

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-ebpf

https://docs.tigera.io/calico/latest/about/kubernetes-training/about-k8s-networking

https://www.tigera.io/blog/everything-you-need-to-know-about-kubernetes-pod-networking-on-aws/

https://www.tigera.io/blog/everything-you-need-to-know-about-kubernetes-networking-on-azure/

https://www.tigera.io/blog/everything-you-need-to-know-about-kubernetes-networking-on-google-cloud/

https://docs.openshift.com/container-platform/4.8/windows_containers/understanding-windows-container-workloads.html#understanding-windows-container-workloads
</pre>


## Please give your training feedback at the below url ( you may use the RPS cloud machine )
<pre>
https://survey.zohopublic.com/zs/CAD36F  
</pre>
