kubectl 分为两种, 一种是命令交互式操作 imperative, 另一种是声明式declarative 通过 configuration file (通常是 yaml 文件)来进行操作. 交互式适合调试的场景, 而声明式适合 IaC 的场景
imperatve 模式
```shell
kubectl create
kubectl update
kubectl delete
kubectl describe # 描述resource info
```

kubectl configuation file 也称为 kubenetes manifests
```shell
# 通过 configuration file 来创建和 update
kubectl apply -f nginx-configuration.yaml
# 通过 configuration file 来删除
kubtctl delete -f nginx-configuration.yaml
```

查询信息
```shell
kubectl describe pod <pod-name>

kubectl port-foward <pod-name> 8080:5001 # 8080 is port in local device


kubectl delete pod -l "app.kubernetes.io/name=grade-submission"

# 
kubectl get endpointslices \
  -l kubernetes.io/service-name=grade-submission-portal

Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME                      ENDPOINTS         AGE
grade-submission-portal   10.244.0.9:5001   58s
```


### kubenetes
```shell
# 帮助
kubectl --help 
kubectl get --help
```
### log
```shell
kubectl logs <pod-name>
# steaming log
kubectl logs -f <pod-name> -c <container-name>
```
### node
```shell
# 查看节点信息
kubectl get node -o wide
```
### namespace
```shell
kubectl get namespace
# 创建 namespace
kubectl create namesapce <namespace_name>
```
### pod
```shell
# check node information
kubectl get node 
# 查看 pod 详细信息, 包好ip地址
kubectl get pods -o wide 
# 查看 namespace 下面 所有 pod 的 label 信息
kubectl get pods -n <namespace> --show-labels
# 通过 label selector 来过滤 pod
kubectl get pods -n beebox-mobile --selector app=auth
NAME           READY   STATUS    RESTARTS   AGE
auth-proc      2/2     Running   0          79m
beebox-auth1   1/1     Running   0          79m
```
### service
```shell
kubectl get svc
kubectl describe service <service_name> 
```
### Deployment

```shell
# 查看 user-db deployment
kubectl get deployment user-db -o yaml
```

### 容器层面的命令
```shell
# 向 pod 中的所有容器下发命令
kubectl exec -it <pod_name> -- <command>
# 向 pod 中指定某个容器下发命令 -c
kubectl exec -it <pod_name> -c <container_name> -- <command>

```
# Draining a node
gracefully terminate the cont

daemonsets: pods that are tied to each node
```shell
kubectl drain <node_name> --ignore-daemonsets --force
kubectl uncordon <node_name> # 关联上

# 通过 pod 文件来构建 pod, 这里的文件也可以是一个 url
kubectl apply -f pod.yml
kubectl apply -f deployment.yml
```


```shell
kubectl delete deployment my-deployment
```

| Command                         | Description                                                               |
| ------------------------------- | ------------------------------------------------------------------------- |
| **for …do**                     | Runs a for command multiple times as specified.                           |
| **kubectl config get-clusters** | Displays clusters defined in the kubeconfig.                              |
| **kubectl config get-contexts** | Displays the current context.                                             |
| **kubectl expose**              | Exposes a resource to the internet as a Kubernetes service.               |
| **kubectl get pods -o wide**    | Lists all the Pods with details.                                          |
| **kubectl get deployments**     | Lists the deployments created.                                            |
| **kubectl get services**        | Lists the services created.                                               |
| **kubectl proxy**               | Creates a proxy server between a localhost and the Kubernetes API server. |
| **kubectl run**                 | Creates and runs a particular image in a pod.                             |
| **kubectl version**             | Prints the client and server version information.                         |

