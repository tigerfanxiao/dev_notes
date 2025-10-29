
# 考试前检查环境
1. 登录base设备, 一般是node1
2. 用户名是 candidate, 密码是 123

查看node和pod状态
- base 上执行也是可以看到所有node的状态的
-  查看版本信息1.32.1 节点个数是3个, 一个master, base是自己, 查看所有pod
```shell
kubectl get nodes
kubectl get pod --A
```

# 第一题 HPA 自动扩缩容 autoscaling 
## 题目需求
> Task 在 autoscale namespace 中创建一个名为 apache-server 的新HorizontalPodAutoscaler(HPA)。此 HPA 必须定位到 autoscale namespace 中名为 apache-server的现有 Deployment 。将 HPA 设置为每个 Pod 的 CPU 使用率旨在 50% 。将其配置为至少有 1 个 Pod，且不超过 4 个 Pod 。此外，将缩小稳定窗口设置为 30 秒
```shell
[candidate@base] $ ssh cka000050
```

1. 创建 autoscale 的命令在 Task/Run Applications/HorizontalPodAutoscaler Walkingthrough 里
```shell
kubectl -n autoscale autoscale deployment apache-server --cpu-percent=50 --min=1 --max=4
```

查看autoscale是否创建成功
```shell
kubectl -n autoscale get horizontalpodautoscaler.autoscaling apache-server
```

2. 修改horizontalPodAutoscaler 缩小稳定窗口为30秒
```shell
kubectl -n autoscale edit horizontalpodautoscalers.autoscaling apache-server
```

增加3行, 复制命令在 Task/Run Application/Horizontal Pot Autoscaling 中, 放在maxReplicas: 4的并列下面
```yaml
behavior:
  scaleDown:
	stabilizationWindowSeconds: 30
```

查看在 autoscale namespace中有 apache-server 这个deployment
```shell
kubectl -n autoscale get deployment apache-server
```
退出cka000050 节点
```shell
exit
```

# 第二题 
>[candidate@base] $ ssh cka000024
如下创建新的 Ingress 资源：名称： echo, Namespace： sound-repeater
使用 Service 端口 8080 在 http://example.org/echo 上公开 echoserver-service Service
可以使用以下命令检查 echoserver-service Service 的可用性，该命令应返回 Hello World ^_^： `candidate@master01:~$ curl http://example.org/echo`


```shell
ssh cka000024
```
查询 nginx
```shell
kubectl get ingressclasses.networking.k8s.io
NAME    CONTROLLER             PARAMETERS   AGE
nginx   k8s.io/ingress-nginx   <none>       82d
```

```shell
vim ingress.yaml
```
在网页上查询 Concepts -> Service,Load Balancing, and Networking,-> Ingress 查询 The Ingress resource
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sound-repeater
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx # 之前查询出来的
  rules:
  - http:
	host: "example.org"
      paths:
      - path: /echo
        pathType: Prefix
        backend:
          service:
            name: echoserver-service
            port:
              number: 8080

```
应用 ingress
```shell
kubectl apply -f ingress.yaml
ingress.networking.k8s.io/echo configured

# 验证解雇
curl http://example.org/echo
Hello World ^_^
# 退回base
exit
logout
Connection to cka000024 closed.
candidate@base:~$
```


# 第三题 Sidecar

>您必须连接到正确的主机。不这样做可能导致零分。
>```shell
[candidate@base] $ ssh cka000037
>```
>
Context
您需要将一个传统应用程序集成到 Kubernetes 的日志架构(例如 kubectl logs)中。实现这个要求的通常方法是添加一个流式传输并置容器。
Task更新现有的 synergy-leverager Deployment，
将使用 busybox:stable 镜像，且名为 sidecar 的并置容器，添加到现有的 Pod 。新的并置容器必须运行以下命令
>```shell
>/bin/sh -c "tail -n+1 -f /var/log/synergy-leverager.log"
>```
>使用挂载在 /var/log 的 Volume，使日志文件 synergy-leverager.log 可供并置容器使用。
>除了添加所需的卷挂载之外，请勿修改现有容器的规范。

解放
```shell
ssh cka000037
```
导出yaml
```shell
kubectl get deployment synergy-leverager -o yaml > sidecar.yaml
```
修改导出的yaml
```shell
vim sidecar.yaml
```

参考链接
依次点击 Concepts → Cluster Administration → Logging Architecture

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: counter
spec:
  containers:
  - name: count
    image: busybox:1.28
    args:
    - /bin/sh
    - -c
    - >
      i=0;
      while true;
      do
        echo "$i: $(date)" >> /var/log/1.log;
        echo "$(date) INFO $i" >> /var/log/2.log;
        i=$((i+1));
        sleep 1;
      done      
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  - name: count-log-1
    image: busybox:1.28
    args: [/bin/sh, -c, 'tail -n+1 -F /var/log/1.log']
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  - name: sidecar
    image: busybox:stable
    args: [/bin/sh, -c, 'tail -n+1 -F /var/log/synergy-leverager.log']
    volumeMounts:
    - name: varlog
      mountPath: /var/log
  volumes:
  - name: varlog
    emptyDir: {}

```
更改后
```shell
candidate@master01:~$ cat sidecar.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"labels":{"app":"synergy-leverager"},"name":"synergy-leverager","namespace":"default"},"spec":{"replicas":1,"selector":{"matchLabels":{"app":"synergy-leverager"}},"template":{"metadata":{"labels":{"app":"synergy-leverager"}},"spec":{"containers":[{"args":["/bin/sh","-c","i=0; while true; do\n  echo \"$(date) INFO $i\" \u003e\u003e /var/log/synergy-leverager.log;\n  i=$((i+1));\n  sleep 5;\ndone\n"],"image":"busybox","imagePullPolicy":"IfNotPresent","name":"synergy-leverager"}]}}}}
  creationTimestamp: "2025-03-02T05:09:39Z"
  generation: 1
  labels:
    app: synergy-leverager
  name: synergy-leverager
  namespace: default
  resourceVersion: "48347"
  uid: ed1ba8bf-aa83-47e7-b04a-f90c0534a6fd
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: synergy-leverager
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: synergy-leverager
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - |
          i=0; while true; do
            echo "$(date) INFO $i" >> /var/log/synergy-leverager.log;
            i=$((i+1));
            sleep 5;
          done
        image: busybox
        imagePullPolicy: IfNotPresent
        name: synergy-leverager
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts: # 给主应用挂在存储和sidecar一致
        - name: varlog
          mountPath: /var/log
      - name: sidecar
        image: busybox:stable
        args: [/bin/sh, -c, 'tail -n+1 -F /var/log/synergy-leverager.log']
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        emptyDir: {}
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2025-03-02T05:09:39Z"
    lastUpdateTime: "2025-03-02T05:09:40Z"
    message: ReplicaSet "synergy-leverager-7c4f9889b4" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  - lastTransitionTime: "2025-05-24T05:10:03Z"
    lastUpdateTime: "2025-05-24T05:10:03Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
candidate@master01:~$

```

应用更新
```shell
kubectl apply -f sidecar.yaml
```

检查新 Pod 是否 Running
```shell
candidate@master01:~$ kubectl get deployment synergy-leverager
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
synergy-leverager   1/1     1            1           83d


candidate@master01:~$ kubectl get pod | grep synergy-leverage
synergy-leverager-69c476cb57-thmkd   2/2     Running   0             57s

```
检查 sidecar 正常打印日志，其中绿色内容，要根据实际的 Pod 名字修改
```shell
kubectl logs synergy-leverager-69c476cb57-thmkd -c sidecar
Sat May 24 05:28:13 UTC 2025 INFO 0
Sat May 24 05:28:18 UTC 2025 INFO 1
Sat May 24 05:28:23 UTC 2025 INFO 2
Sat May 24 05:28:28 UTC 2025 INFO 3
Sat May 24 05:28:33 UTC 2025 INFO 4
Sat May 24 05:28:38 UTC 2025 INFO 5
Sat May 24 05:28:43 UTC 2025 INFO 6
Sat May 24 05:28:48 UTC 2025 INFO 7
Sat May 24 05:28:53 UTC 2025 INFO 8
Sat May 24 05:28:58 UTC 2025 INFO 9
Sat May 24 05:29:03 UTC 2025 INFO 10
Sat May 24 05:29:08 UTC 2025 INFO 11
Sat May 24 05:29:13 UTC 2025 INFO 12
Sat May 24 05:29:18 UTC 2025 INFO 13
Sat May 24 05:29:23 UTC 2025 INFO 14
Sat May 24 05:29:28 UTC 2025 INFO 15
Sat May 24 05:29:33 UTC 2025 INFO 16
Sat May 24 05:29:38 UTC 2025 INFO 17
Sat May 24 05:29:43 UTC 2025 INFO 18
Sat May 24 05:29:48 UTC 2025 INFO 19
Sat May 24 05:29:53 UTC 2025 INFO 20
Sat May 24 05:29:58 UTC 2025 INFO 21
Sat May 24 05:30:03 UTC 2025 INFO 22
Sat May 24 05:30:08 UTC 2025 INFO 23

```
退出 cka000037 节点， 返回到 candidate@base 用户和节点
```shell
exit
logout
Connection to cka000037 closed.

```

# 第四题 StorageClass

>您必须连接到正确的主机。不这样做可能导致零分。
>`[candidate@base] $ ssh cka000046`
Task
首先，为名为 rancher.io/local-path 的现有制备器，创建一个名为 ran-local-path 的新 StorageClass将卷绑定模式设置为 WaitForFirstConsumer
注意，没有设置卷绑定模式，或者将其设置为 WaitForFirstConsumer 之外的其他任何模式，都将导致分数降低。
接下来，将 ran-local-path StorageClass 配置为默认的 StorageClass
请勿修改任何现有的 Deployment 和 PersistentVolumeClaim，否则将导致分数降低。

依次点击 Concepts → Storage → Storage Classes → StorageClass objects
https://kubernetes.io/docs/concepts/storage/storage-classes/

```shell
# 确保在 candidate@base:~$下，再执行切换集群的命令
ssh cka000046
```

```shell
vim sc.yaml
```
参考
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ran-local-path # 修改名字
  annotations:
    storageclass.kubernetes.io/is-default-class: "true" # 修改为true
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
```
应用配置
```shell
candidate@master01:~$ kubectl apply -f sc.yaml
storageclass.storage.k8s.io/ran-local-path created
```
检查, 出现default 才是对的
```shell
candidate@master01:~$ kubectl get storageclass
NAME                       PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path                 nfs-client              Delete          Immediate              true                   83d
ran-local-path (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  14s
```

退出
```shell
exit
```
# 第五题 Service

>[candidate@base] $ ssh cka000022
Task
重新配置 spline-reticulator namespace 中现有的 front-end Deployment，以公开现有容器 nginx 的端口 80/tcp创建一个名为 front-end-svc 的新 Service ，以公开容器端口 80/tcp
配置新的 Service ，以通过 NodePort 公开各个 Pod

```shell
 ssh cka000022

```

```shell
kubectl -n spline-reticulator edit deployment front-end

```

```yaml
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: front-end
    spec:
      containers:
      - image: vicuu/nginx:hello
        imagePullPolicy: IfNotPresent
        name: nginx
        ports: # 新增
        - containerPort: 80 # 新增
          protocol: TCP # 新增
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30

```


```shell
kubectl -n spline-reticulator expose deployment front-end --type=NodePort --port=80 --target-port=80 --name=front-end-svc
service/front-end-svc exposed
candidate@master01:~$ kubectl -n spline-reticulator get svc front-end-svc -o wide
NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE   SELECTOR
front-end-svc   NodePort   10.110.217.255   <none>        80:31782/TCP   16s   app=front-end

```


```shell
candidate@master01:~$ curl 10.110.217.255:80
Hello World ^_^

```


# 第七题

先登录 argo-helm的官网
```shell
https://argoproj.github.io/argo-helm
```

# 第九题

>`[candidate@base] $ ssh cka000056`
Task
从提供的 YAML 样本中查看并应用适当的 NetworkPolicy。
确保选择的 NetworkPolicy 不过于宽松，同时允许运行在 frontend 和 backend namespaces 中的 frontend 和 backend Deployment 之间的通信。
首先，分析 frontend 和 backend Deployment，以确定需要应用的 NetworkPolicy 的具体要求。
接下来，检查位于 ~/netpol 文件夹中的 NetworkPolicy YAML 示例。
注意：请勿删除或修改提供的示例。仅应用其中一个。否则可能会导致分数降低。
最后，应用启用 frontend 和 backend Deployment 之间的通信的 NetworkPolicy，但不要过于宽容。
注意：请勿删除或修改现有的默认拒绝所有入站流量或出口流量 NetworkPolicy。否则可能导致零分