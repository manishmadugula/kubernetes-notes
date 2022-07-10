## Kubectl CLI
```bash
    alias k=‘kubectl’
    alias kgp=‘kubectl get pods’
    alias kgs=‘kubectl get service’
    alias kd=‘kubectl delete’
    alias kcf=‘kubectl create -f’
    alias kaf=‘kubectl apply -f’
    alias kgpa=‘kubectl get pods --all-namespaces’
    alias l=ls
    alias sl=ls

    vi ~/.vimrc
    set number
    set tabstop=2
    set expandtab
    set shiftwidth=2
    set cursorline
```

- ```kubectl delete pods pod_name --grace-period=0 --force```
    - ```kubectl delete pod unwanted --now```
- ```kubectl get pods pod_name -o yaml --export```
- make sure not to type ctrl+w (Use Extension)
- Find a ctrl-w alternate
- ```kubectl get pod my-pod -o yaml > pod.yaml```
- ```kubectl explain pods.spec.containers | grep -C 10 envFrom```
-Recursively explain ```kubectl explain pods.spec.containers --recursive | grep -C 10 envFrom```
- Understand the explain command very well.
- Learn how to copy paste from terminal to vim and also from vim to vim and also from documentation to terminal and vim.
- Know the short forms
    ```bash
        - svc (Services)
        - sa (serviceAccounts)
        - cm (configMap)
        - ns (namespaces)
        - pvc
        - pv
        - deploy
    ```
- ```k exec -it web-dashboard-5899cf7849-k66xn ls /var/run/secrets```
- Dry run to get the pod def file and edit it before deploying.```k run mosquito --image=nginx --dry-run -o yaml>pod.yaml```
- ```k explain pod.spec --recursive | grep -i -C 10 containerport```
- Autocomplete is enabled in the exam, but make sure to enable autocomplete with alias by running the following
    ```bash
    alias k=kubectl
    complete -F __start_kubectl k
    ```
### Useful Linux commands
- grep
- wc
- vi
- 


### Viewing all resources
```k get all -l env=prod```

### Updating existing resources

#### Pod
- A lot of pod properties are not editable. In those cases simply edit the yaml file, it will throw error and then save to ```/tmp/random.yaml```. You can now either force delete the pod and recreate the pod using the yaml at ```/tmp``` 
 ```k edit pod nginx```
 ```kubectl delete pods nginx --grace-period=0 --force```
 This might be simpler``kubectl delete pod unwanted --now```
- or you can replace using a single line command
```k replace --force -f /tmp/kubectl-edit-8572845.yaml```

#### Container name
- ```k set pod/podname current_image:new_image```
- ```k set deployment/deploymentname current_image:new_image```


### Monitoring
- ```k logs pod-name -n namespace-name```

### Debugging
- describe command.

## Common Mistakes
- ```kubectl create secret generic``` (Don't forget generic)

## Yaml
- Diff between object and array very important

## Linux Commands
- Ctrl+k (Remove everything till the end of the line)
    - Can be used as an alternative to Ctrl+w (Go to the desired word using Ctrl+left arrow and then Ctrl+k)
- Ctrl+r (History loop over)
- Ctrl+u (Remove everything till start of the line)
- Ctrl+z inside vim (send to background)
- fg (get it back from background)
- tmux
- | grep -inC 5 keywordToSearch
- (Enabled by default) : source < (kubectl completion bash)
### Useful workflows
#### For working with yaml files
- ```bash
    export dry="--dry-run=client -o yaml"

    k create deployment demo --image=nginx \
    $dry > demo.deployment.yaml
    ```
    
- ```bash
    cat <<eof >pv.yaml
    [PASTE HERE]
    eof

    vim pv.yaml

    kubectl create -f pv.yaml
    ```

- ```bash
    #Say you are inside vim
    CTRL + Z ## Send vim to background
    #[
      #  ANY SEQUENCE OF SHELL COMMANDS, say explain or --help
    #]
    fg ## Back to vim
  ```


## VIM Commands
- ```gg->First```
- ```G->Last```
- ```u ->Undo```
- ```dd ->Delete entire line```

## SSH
- Learn SSH

## Cheat Sheet Immediate Finds
- First paste the alias and enable auto completion
```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
alias k=kubectl
complete -F __start_kubectl k
```

- search set-context (Change namespace to one in the questions)
```
kubectl config set-context --current --namespace=ggckad-s2
```

- search stdin (Go inside the pod) 
```
kubectl exec --stdin --tty my-pod -- /bin/sh
```


## Review
- https://www.youtube.com/watch?v=4yhdTz1NFU0
- Linux Foundation Exam Environment Review Video
- Udemy Course complete.
- CKAD Simulator : killer.sh

### Copy and Pasting
- For Windows: Ctrl+Insert to copy and Shift+Insert to paste

### Time Management
```bash
 kubectl create deployment nginx --image=nginx   (deployment)
kubectl run nginx --image=nginx --restart=Never   (pod)
kubectl run nginx --image=nginx --restart=OnFailure   (job)  
kubectl run nginx --image=nginx  --restart=OnFailure --schedule="* * * * *" (cronJob)

kubectl run nginx -image=nginx --restart=Never --port=80 --namespace=myname --command --serviceaccount=mysa1 --env=HOSTNAME=local --labels=bu=finance,env=dev  --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi' --dry-run -o yaml - /bin/sh -c 'echo hello world'

kubectl run frontend --replicas=2 --labels=run=load-balancer-example --image=busybox  --port=8080
kubectl expose deployment frontend --type=NodePort --name=frontend-service --port=6262 --target-port=8080
kubectl set serviceaccount deployment frontend myuser
kubectl create service clusterip my-cs --tcp=5678:8080 --dry-run -o yaml
```