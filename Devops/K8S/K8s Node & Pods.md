
# Node Operations
add label to node

```shell
kubectl label nodes k8s-worker2 <label_key>=<label_value>
kubectl get nodes --show-labels 
```

# Pod Operations
view the pod
```shell
kubectl get pods -n <name-space>
```

create pod by file
```shell
kubectl create pod -f <file.yml>
```

delete the pod

```shell
kubectl delete pod <pod_name> -n <name_space>
```

assign pod on specific node
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: auth-gateway
  namespace: beebox-auth
spec:
  nodeSelector: # 指定 node
    <label_key>: <label_value>
  containers:
  - name: nginx
    image: nginx:1.19.1
    ports:
    - containerPort: 80
```

在 deployment中操作
```yaml
kind: Deployment
metadata:
  name: auth-data
  namespace: beebox-auth
spec:
  replicas: 3
  selector:
    matchLabels:
      app: auth-data
  template:
    metadata:
      labels:
        app: auth-data
    spec:
      nodeSelector: # 选择这个标签的 node
        external-auth-service: "true"
      containers:
      - name: nginx
        image: nginx:1.19.1
        ports:
        - containerPort: 80
```


# busybox
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-svc-test-new-namespace
  namespace: new-namespace
spec:
  containers:
  - name: busybox
    image: radical/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 10; done']

```