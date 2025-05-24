
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