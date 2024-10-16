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
