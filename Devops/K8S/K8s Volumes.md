常见的有两种 volume
- hostPath
- shared volume

HostPath 类型的 volume 会在 node 上建立一个目录挂载到 pod 中
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: maintenance-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']
    volumeMounts:
    - name: output-vol
      mountPath: /output 这是pod上的目录
  volumes:
  - name: output-vol
    hostPath:
      path: /var/data # 这是node上目录
```

pod 共享的 volume 应该是临时的. 在 pod 删除后也会自动删除
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: maintenance-pod
spec:
  containers:
  - name: busybox1
    image: busybox
    command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']
    volumeMounts:
    - name: shared-vol
      mountPath: /output
  - name: busybox2
    image: busybox
    command: ['sh', '-c', 'while true; cat /input/output.txt; sleep 5; done']
    volumeMounts:
    - name: shared-vol
      mountPath: /input
  volumes:
  - name: shared-vol
    emptyDir: {}
```