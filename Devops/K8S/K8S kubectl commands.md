
# Deployment

```shell

# 查看 user-db deployment
# 查看 deployment label
kubectl get deployment user-db -o yaml

```


# namespace
```shell
# 创建 namespace
kubectl create namesapce <namespace_name>
```

```shell
kubectl get node # check node information
kubectl get service
kubectl get svc # get the service

# 查看节点信息
kubectl get node -o wide
# 查看 pod 信息
kubectl get pods -o wide 
NAME           READY   STATUS    RESTARTS   AGE   IP               NODE          NOMINATED NODE   READINESS GATES
auth-proc      2/2     Running   0          80m   192.168.194.68   k8s-worker1   <none>           <none>
beebox-auth1   1/1     Running   0          80m   192.168.194.66   k8s-worker1   <none>           <none>
beebox-auth2   1/1     Running   0          80m   192.168.194.65   k8s-worker1   <none>           <none>

# 查看 lable 信息
kubectl get pods -n beebox-mobile --show-labels
NAME           READY   STATUS    RESTARTS   AGE   LABELS
beebox-auth2   1/1     Running   0          68m   app=auth
db-proc        2/2     Running   0          68m   app=db

# 使用 selector
kubectl get pods -n beebox-mobile --selector app=auth
NAME           READY   STATUS    RESTARTS   AGE
auth-proc      2/2     Running   0          79m
beebox-auth1   1/1     Running   0          79m

kubectl --help # 
kubectl get --help


kubectl describe service <service_name> 

kubectl logs <podname>
kubectl logs <podname> -f # stream the log


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
| **kubectl apply**               | Applies a configuration to a resource.                                    |
| **kubectl config get-clusters** | Displays clusters defined in the kubeconfig.                              |
| **kubectl config get-contexts** | Displays the current context.                                             |
| **kubectl create**              | Creates a resource.                                                       |
| **kubectl describe**            | Shows details of a resource or group of resources.                        |
| **kubectl expose**              | Exposes a resource to the internet as a Kubernetes service.               |
| **kubectl get**                 | Displays resources.                                                       |
| **kubectl get pods**            | Lists all the Pods.                                                       |
| **kubectl get pods -o wide**    | Lists all the Pods with details.                                          |
| **kubectl get deployments**     | Lists the deployments created.                                            |
| **kubectl get services**        | Lists the services created.                                               |
| **kubectl proxy**               | Creates a proxy server between a localhost and the Kubernetes API server. |
| **kubectl run**                 | Creates and runs a particular image in a pod.                             |
| **kubectl version**             | Prints the client and server version information.                         |

```shell


# 向 pod 中的所有容器下发命令
kubectl exec -it <pod_name> -- <command>
# 向 pod 中指定某个容器下发命令 -c
kubectl exec -it <pod_name> -c <container_name> -- <command>

```