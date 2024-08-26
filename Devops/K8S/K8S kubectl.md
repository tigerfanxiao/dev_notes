
```shell
kubectl get node # check node information
kubectl get service
kubectl get svc # get the service

# 查看节点信息
kubectl get node -o wide
kubectl get pods -o wide 

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
kubectl uncordon <node_name>

# 通过 pod 文件来构建 pod
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
|                                 | Deletes resources.                                                        |
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