# Install kubernetes locally
- Install kubectl add to path
- Install minikube
- Make sure virtualization is enabled
- Install virtual Box
- ```bcdedit /set hypervisorlaunchtype off``` and restart
- ```minikube start --driver=virtualbox  --no-vtx-check```

## Delete minikube cluster
- ```minikube delete --all```

## Create a hello world deployment and expose a service
- ```kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10```
- ```kubectl expose deployment hello-minikube --type=NodePort --port=8080```