# Day4

## Docker Network Model
<pre>
- Each container gets an unique private IP on the system it runs
- Containers can be connected to Docker Network
- Docker creates a default bridge network called docker0 with subnet 172.17.0.0/16 ( 65535 IP addresses )
- Containers get their IP address from the subnet range assigned to the Docker network
- Containers running on same network can communicate with each other directly
- a single container can be connected to multiple networks, which means they may get multiple IP addresses
</pre>  

#### Subnet
<pre>
- 172.17.0.0/16
- IPV4 IP address
- 172 - 1 byte
- 17 - 1 byte
- 0 - 1 byte
- 0 - 1 byte
- IPv4 - 4 bytes ( 32 bits )
- CIDR - Classless Inter Domain Routing
- First IP in 172.17.0.0/16 - 172.17.0.0
- 172.17.0.1
- 172.17.0.255
- 172.17.1.0
- 172.17.1.255
- 256 x 256 = 65535 IP addresses are there in 172.17.0.0/16 Network
</pre>

## Kubernetes Network Model
<pre>
- Kubernetes doesn't implement Network, it only publishes its Network requirements as Kubernetes Network specification, while third-party vendors implements the Kubernetes Network specification, which is referred as Kubernetes Network Model
- Each Pod should get an unique cluster wide IP address within the Kubernetes cluster
- Pod Network
  - all pods should be able to communicate with each other whether they run in same node or different nodes
  - kubelet should be able to communicate with the pods that runs on the node where kubelet is running
- Service Network
  - Pods are temporary as they get removed in the process of scale up/down, rolling update, etc.
  - application developers should not use Pod IP to access them
  - every service should get an unique cluster wide name and IP address
  - service IP once assigned to a service will not change until the service exists
  - i.e service IP is stable
  - service represents a group of Load Balanced Pods behind them
- Ingress
  - makes services accessible outside the Kubernetes cluster
- Network Policy
  - helps controlling communication between Pods
  - helps controlling Pod incoming/outgoing traffic
</pre>

## Kubernetes Network CNI Addons
<pre>
- the Kubernetes Network Model(Specification) is implemented by third-party CNI plugins/addons
- Some of the Kubernetes CNI Network addons are
  - Calico
  - Weave Net
  - Flannel
  - Antrea
  - Canal
  - Cilium
  - Gateway API
  - Multus
  - Romana
  - Spiderpool
  - Nuage
  - NST-T
  - Knitter
  - OVN-Kubernetes
  - Nodus
  - Contrail
  - Contiv
  - ACI
- Popular addons most commonly used by many companies
  - Flannel
  - Calico
  - WeaveNet
</pre>

### Flannel
You may find this article interesting 
<pre>
https://mvallim.github.io/kubernetes-under-the-hood/documentation/kube-flannel.html  
</pre>

Let's understand flannel briefly
<pre>
- is a simple layer 3 network fabric designed for Kubernetes by CoreOS organization
- when flannel CNI is installed in Kubernetes, one Flannel Pod gets created on each Kubernetes node
- flannel either uses Kubernetes API to store network configurations in etcd or gets direct write access to etcd database just like API Server
- when kubernetes master node is bootstrapped(installed), we need to assign a Pod network subnet
- eg: kubeadm init --pod-network-cidr=10.244.0.0/16
- in the above example 10.244.0.0/16 ( 65535 IP addresses are allocated for Pods created in the K8s cluster )
- the above subnet is sub-divided into small subnets(IP ranges) and allocated to every node by Flannel
  - For example
    - Master 1 - 10.244.1.0/24 ( Upto 256 IP addresses, as Nodes by default can support only 110 Pods this is more than enough )
    - Master 2 - 10.244.2.0/24 
    - Master 3 - 10.244.3.0/24
    - Worker 1 - 10.244.4.0/24
    - Worker 2 - 10.244.5.0/24
    - Worker 3 - 10.244.6.0/24
- the subnet for every node is    
- Flannel supports multiple backends for encapsulating packets
  - VXLAN ( recommended )
  - host-gw
- Flannel doesn't support Network Policy
</pre>

### Calico
<pre>
-   
</pre>

### Weave
