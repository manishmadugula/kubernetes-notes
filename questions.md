- What is the difference between
```yaml
spec:
    containers:
        - name: ubuntu
          command:
            - sleep
          args:
            - 5000
```
and 
```yaml
spec:
    containers:
        - name: ubuntu
          command: ["sleep"]
          args: ["5000"]
```

- Why is /bin/sh used in command argument of the container defination?