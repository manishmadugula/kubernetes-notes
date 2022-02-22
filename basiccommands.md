## Nodes

### Getting details on nodes
- Get total nodes in cluster
- Get os on which the nodes are running
- Roles of each node
- container run time
- ```kubectl get nodes```
- ```kubectl get nodes -o wide```

## Pods

### Running a pod

#### Using run command
```kubectl run nginx --image=nginx```

#### Using yaml file
- The following are required fields: (apiVersion, kind, metadata and spec)
- A pod-defination.yml file may look as.
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-pod
    labels:
        app: my-app
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
```
- It is deployed using

    ```kubectl create -f pod-defination.yml```
- To update, use this (This can be used always?????, need to check)
    ```kubectl apply -f pod-defination.yml```
- Can also edit the pod yaml directly by:

    ```kubectl edit pod podname```
    - Make your edits and save that will update the configuration
- To dry run and create a yaml without deploying the pod
    ```kubectl run redis --image=redis --dry-run=client -o yaml > pod.yaml```

### Getting the status of the pods

- ```kubectl get pods```
- ```kubectl get pods -o wide```
- ```kubectl describe pods```
- ```kubectl describe pod podname```

### deleting a pod

### Go inside the pod
```kubectl exec --stdin --tty podname -- /bin/bash```

## Replication Set
- Process that monitors the pod.
- It uses matchLabels property to know which pods to monitor.
- It has different apiVersion than others its : apps/v1
- Selector is one of the major difference between ReplicaSet and ReplicaController(deprecated)
- It needs 3 distinct section in yml file
    - template
    - selector
    - replicas

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginxreplicaset
  labels:
    app: myapp
    env: dev
spec:
  template:
    metadata:
      name: nginxpod
      labels:
        app: myapp
        env: dev
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector:
    matchLabels:
      app: myapp
```

### How to scale replica set
- ```kubectl replace -f replicaset-defination.yml``` or
- ```kubectl scale --replicas=6 -f replicaset-defination.yml``` or
- ```kubectl scale --replica=6 replicaset replica-set-name```
- ```kubectl edit replicaset replicaset_name```


## Deployments

- Most commands as well as yaml defination are same as ReplicaSet
- Rolling update is the default update strategy

### Create deployments
- ```kubectl create deployment deployment_name --image=nginx```
- ```kubectl scale deployment deployment_name --replicas=3```

### Rollout Status

```kubectl rollout status deployment/myapp-deployment```

### Rollout History
```kubectl rollout history deployment/myapp-deployment```

### Rollback
```kubectl rollout undo deployment/myapp-deployment```