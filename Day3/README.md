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
