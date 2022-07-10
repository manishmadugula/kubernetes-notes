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
- nodePort, targetPort, port are from the viewpoint of the service.
- To have services in differnet namespaces, create ingresses in different namespaces, if you create a path which is same, then you get the error, do the lab on udemy on ingress1.
- What is the rewrite-target option?
- Ingress controller requires configmap, it's own namespace, service account, role, clusterrole, role-binding, cluster-role binding, service and finally the deployment with proper image.
- Ingress resource needs to be in the namespace where the app-pods are there.


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

## Namespaces
- set namespace kubectl config set-context --current --namespace=<insert-namespace-name-here>
- Avoid creating namespaces with the prefix kube-, since it is reserved for Kubernetes system namespaces.
### Items not in any namespace
``` kubectl api-resources --namespaced=false```
- nodes
- pv
- clusterrole
- clusterrolebinding
- storageclasses

## Pods

### InitContainers
- If you specify multiple init containers for a Pod, kubelet runs each init container sequentially. Each init container must succeed before the next can run. When all of the init containers have run to completion, kubelet initializes the application containers for the Pod and runs them as usual.
#### Get logs of initcontainer
```k logs pod -c container_name```

### Labels and Selectors
Get pods with label
```k get pods -l env=dev```
```k get pod -l env=prod,bu=finance,tier=frontend```

## Deployments
### Rollout
```k rollout status deployment/deployment_name```
```k rollout history deployment/deployment_name```
```k rollout undo deployment/deployment_name```
```kubectl rollout undo deployment/frontend --to-revision=2         # Rollback to a specific revision```
- On undo, the older revision to which you have undoed to will be edit in history with incremented verision.