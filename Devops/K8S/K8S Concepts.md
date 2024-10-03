
### K8S Component and Objects

- node 每一个物理机或者实体机可以看做为一个 node
	- 至少有还一个管理节点 Master node, 1 个或者多个 worker node
	- 多个容器可以共享同一个 pod 中的一片存储
- kubelet 在每一个 node 上的 pods 都由 kubelet 来管理运行
- kubeadm 用于管理 kubernetes 版本升级, 创建集群 cluster
- etcd 是一个 key-value 数据库, holds the current status of k8s component
- API Server: Entrypoint to K8S cluster 管理员可以通过 UI, API 或者 命令行Kubectl 来访问 K8S
- Scheduling k8s把 pods 分配到不同node 的过程
	- Scheduler 在控制平面确保 pod 的provision在哪个 Node 上, 每个 pod 使用多少资源
- Pod k8s 的最小单位. 至少含有一个或者多个 container
- RBAC Role based access account
	- ServiceAccount: 每个 pod 默认都会有一个 service account 与 k8s 的 api 进行交互
- ConfigMap 因为 container 是无状态的. 所以是configMap 来向 pod 传递配置信息
- Secret 用于向 pod 传递密码信息
- DaemonSet 规定了每个 node 都会有一个 pod replica
- Deployment 使用 template 来规定 pod 创建完成的状态
- ReplicaSet
- StatefulSet 用于管理数据库这些 stateful application
- Volume 用户数据持久化

# Best Practice

经典的用法是讲 Nginx和日志读取工具放在同一个 pod 中. 当 nginx 在写日志的时候, 日志读取工具就能读日志. 每一个 Pod 有一个 IP 地址

容器运行时 container runtime 就是正在运行的 docker. K8S 不再支持 docker 的运行时, 表示我们还是可以用 docker compose 文件去别写 docker 的配置. 然后用 containerd 去拉起容器, 形成一个进行时

Node 是计算节点, 可以是物理机, 也可以是虚拟机


### Service
Service 有 Service 的 IP 地址, 由 Service的 IP 地址来接受请求, 然后做负载均衡, 发送给 Pod
Service 有两种IP
- Cluster IP 接受 Cluster 内部的流量
- Node IP 接受 Cluster 外部的流量

### Istio

Istio 是一种 K8S 生态中的一部分

# k8s Management Tools
Kompose 把 docker compose 文件变成 k8s 对象
Helm  k8s 配置工具 template
Kustomize k8s 配置工具 template