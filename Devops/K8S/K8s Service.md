一共用 4 中 service
- ClusterIP service 处理内部流量
- NodePort service 处理外部流量
- Load Balancer Service
- Cloud sevice

### ClusterIP Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-clusterip
spec:
  type: ClusterIP # 只在 cluster 内部的流量
  selector: 
    app: svc-example # 把流量引导 app: svc-example label 的 development
  ports:
    - protocol: TCP
      port: 80 # service 对外部开放的端口是 80
      targetPort: 80 # deployment 的 pod 开放的端口是 80

```

```shell

# 查看创建的 svc
kubectl get svc
# 可以看到pod 的 ip 地址
kubectl get endpoints svc-clusterip

kubectl exec busypod -- curl user-app-svc
```

### NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-nodeport
spec:
  type: NodePort
  selector: 
    app: svc-example
  ports:
    - protocal: TCP
      port: 80 # 内部 service 的端口
      targetPort: 80 # pod 的端口
      nodePort: 30080 # 外部流量访问这个监听端口, IP地址可以是 control node, 也可以是 work node
```

可以用浏览器访问 30080端口
```shell
curl localhost:30080
```

# Service DNS

`service-name.namespace-name.svc.cluster-domain.example`
The default cluster domain is cluster.local
可以跨 namespace通信, 如果使用 fully qualified domain name
如果是同一个 namespace

通过 nslookup 反向查阅 fully qualified domain name
```shell
kubectl get service <service_name>
kubectl exec busybox -- nslookup 10.103.1.1
```


```shell
kubectl exec -n new-namespace pod-svc-test-new-namespace --curl svc-clusterip.default.svc.cluster.local
```

# Ingress 
ingress是比 service 有更多的安全特性, 放在 service 的前面
- SSL termination
- Advanced Load Balancing
- name-based virtual hosting
在 ingress 中设置了 routing rule, 可以控制说那些请求受routing rule 控制
Each rule has a set of paths, each with a backend. Request matching  a path will be routed to its associated backend. 

用人话来说, 就是去往某个 path 的流量, 会被 route rule 引导某个 service 的某个端口
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - http:
      paths: /somepath
      pathType: Prefix
      backend:
        service:
          name: my-service
          port: 
            number: 80
```
