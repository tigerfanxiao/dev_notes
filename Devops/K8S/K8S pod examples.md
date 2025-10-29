

### nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
sepc:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort:80
  restartPolicy: OnFailure
```


busybox

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-svc-test
spec:
  containers:
  - name: busybox
    image: radical/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 10; done'] # 迫使这个容器一直开着
```

在这个容器中执行命令
```shell
# 向 service 发送请求
kubectl exec pod-svc-test -- curl svc-clusterip:80
# 应该只有 service 中的一个 pod 回应了
```