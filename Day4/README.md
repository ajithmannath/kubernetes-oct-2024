# Day4

## Docker Network Model

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
- 
</pre>

### Calico

### Weave
