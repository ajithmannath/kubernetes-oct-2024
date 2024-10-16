# Day 3

## Lab - Declaratively creating a clusterip internal service
```
kubectl expose deploy/nginx --type=ClusterIP --port=80 -o yaml --dry-run=client
kubectl expose deploy/nginx --type=ClusterIP --port=80 -o yaml --dry-run=client > nginx-clusterip-svc.yml
kubectl apply -f nginx-clusterip-svc.yml

kubectl get svc
kubectl describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/e967a721-89b7-48f0-b166-a554194e9cce)

## Lab - Declaratively creating nodeport external service
Let's delete the clusterip service in declarative style
```
kubectl delete -f nginx-clusterip-svc.yml
```

Let's create the nodeport external service in declartive style
```
kubectl expose deploy/nginx --type=NodePort --port=80 -o yaml --dry-run=client
kubectl expose deploy/nginx --type=NodePort --port=80 -o yaml --dry-run=client > nginx-nodeport-svc.yml
kubectl apply -f nginx-nodeport-svc.yml

kubectl get svc
kubectl describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/81424d97-6fbf-4375-91c0-13f36acc84d9)


## Lab - Creating a Pod in declarative style

Create a file with below content and save the file as pod.yml
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: frontend
spec:
  containers:
  - name: my-container
    image: tektutor/spring-ms:1.0
```

Let's create the pod
```
kubectl create -f pod.yml --save-config
kubectl get po
```

Let's modify the pod.yml as shown below
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: web
    tier: frontend
spec:
  containers:
  - name: my-container
    image: tektutor/spring-ms:1.0
```

Let's apply the delta changes on the already existing kubernetes resource
```
kubectl apply -f pod.yml
kubectl get po
```

Once you are done with this exercise, you may delete it as shown below
```
kubectl delete -f pod.yml
```

Expected output
![image](https://github.com/user-attachments/assets/6d675cde-4208-4e6a-8211-889d26347ec9)
![image](https://github.com/user-attachments/assets/30e02cb2-8768-48af-a07c-eabb978f9b67)

## Lab - Deploying your custom application into Kubernetes cluster

We need to clone the source code first
```
cd ~
git clone https://github.com/tektutor/spring-ms.git
rm *.yml *.yaml
```

Let's build the custom docker image
```
docker build -t tektutor/hello-spring-microservice:1.0 .
docker images
docker login
docker push tektutor/hello-spring-microservice:1.0 
```

Let's deploy our custom application into K8s cluster
```
kubectl create deployment hello --image=tektutor/hello-spring-microservice:1.0 ---relicas=2
kubectl get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/20598ff3-cd29-494b-9a3d-e8d24e346412)
![image](https://github.com/user-attachments/assets/5045cc3c-9847-4513-8e51-6004fa48d135)
![image](https://github.com/user-attachments/assets/73246a9d-f25c-4d8e-8cf4-8d546ec3efbc)
![image](https://github.com/user-attachments/assets/959cb238-8594-4c21-80d0-9541cebcb17a)

## Lab - Creating a loadbalancer service in declarative style

Let's create an external loadbalancer service for hello deployment
```
kubectl expose deploy/hello --type=LoadBalancer --port=8080 -o yaml --dry-run=client
kubectl expose deploy/hello --type=LoadBalancer --port=8080 -o yaml --dry-run=client > hello-lb-svc.yml
kubectl apply -f hello-lb-svc.yml
kubectl get svc
```

But in order for the loadbalancer service to acquire an external IP, the kubernetes administrator has to install an operator called MetallB.  The Metallb operator configures our local k8s cluster to work like it works in AWS/Azure or any public cloud.

Each time someone creates a LoadBalancer service, the metallb controller will be watching, when it detects some one creating a new loadbalancer service, it configures the metallb load balancer to route the traffic to our pods just like how it works in AWS/Azure.

Let's install the metallb operator
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```

## Lab - Let's add a new type of resource to K8s cluster
Create a file named training-crd.yml with the below content
```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: trainings.tektutor.org
spec:
  group: tektutor.org
  scope: Namespaced
  names:
    kind: Training
    listKind: TrainingList
    plural: trainings
    singular: training
    shortNames:
    - train
  version:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          training:
            type: string
          duration:
            type: string
          from: 
            type: string
          to:
            type: string
```

Let's create a training resource
```
apiVersion: tektutor.org/v1
kind: Training 
metadata:
  name: devops-training 
spec:
  training: "Advanced DevOps"
  duration: "5 Days" 
  from: "4th Nov 2023"
  to: "8th Nov 2023"
```

## Info - Kubernetes Operator Overview
<pre>
- Kubernetes Operator helps us extend the Kubernetes API or used to add new functionality to Kubernetes
- Operator is a combination of one or more Controllers and Custom Resources
</pre>


## Demo - Configure the metallb operator
```
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: default
  namespace: metallb-system
spec:
  ipAddressPools:
  - tektutor-metallb-addresspool
```
