# Important Linux Commands
- Know OS Version ```cat /etc/os-release```
- IP in Linux ```hostname -I```, ```ifconfig```

# Minikube
- SSH inside minikube ```minikube ssh```
- IP of minikube ```minikube ip```
- Good to read [link](https://stackoverflow.com/questions/56433289/why-does-virtualbox-create-2-interfaces-on-host-only-adapter)
- Directly get the service url ```minikube service service-name --url```

# Networking
- K8s assumes that the admins would make sure :
    - All containers and pods should communicate with each other without NAT
    - All nodes should be able to communicate with each other and with containers and pods.
    - We can use solutions like Cisco ACI Networks, Cilium, Flannel, VMWare NSXT etc.
- All containers in a pod would have the same IP.
- Pod IP would be a separate IP than node.
- It is responsibility of admins to make sure IPs of all pods are different in a cluster, no matter the node.

## Example configuration
```
Public IP of Router : 49.207.227.235
My Private IP : 192.168.0.105
VM IP (Minikube) : 192.168.59.101
Pod IP : 172.17.0.3
```

## Services
- Enables communication between various components.
- Enables loose coupling between microservices.
- Built in load balancer across different pods.
- Uses Random assignment with session affinity.
### NodePort Service
- Listen to a port on the node, and forward that request to a port in the pod.
- If we don't provide targetPort, then targetPort is assumed to be same as port.
- If no nodePort, then a valid port in the range is assigned.
- Only port is mandatory field.
Also ports is an array.
![](../Resources/nodeport.png)
### Cluster IP


### LoadBalancer (Supported Cloud Providers)