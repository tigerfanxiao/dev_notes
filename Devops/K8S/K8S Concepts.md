# K8S Architecture

- 至少有一个 Master Node, 和多个 worker node
- Master Node 输入 control plane 也称为管理平面
- Node 可以理解是一个物理机, 或者一台虚拟机
- 每一个 worker node 上都有Kubelet(K8S Process), 用于不同的 cluster 之间的通信
- 每个worker node 上都有多个 Pod, Pod 是 K8S 最小的管理单元. 一个 Pod 中可以有多个容器. 一个 Pod 中的多个容器可以共享同一片存储. 
- 在生产环境中, 至少有两个 master node, 因为一旦 master node 挂了, 就不能访问 cluster 了

### Controler Plan Components
- API Server: Entrypoint to K8S cluster 管理员可以通过 UI, API 或者 命令行Kubectl 来访问 K8S
- Controller Manager: 会检查当前cluster 的状态是否是我们定义好的状态
- Scheduler 确保 pod 的provision在哪个 Node 上, 每个 pod 使用多少资源
- etcd 是一个 key-value 数据库, holds the current status of k8s component
### Main K8S Components

Pod
Configmap 放 URL of database
Service: 处理公网来的流量
Ingress: router traffic into cluster
Secret: 
Deployment 用于 template for creating pods
StatefulSet 用于管理数据库这些 stateful application
ReplicaSet 
DaemonSet
Volume 用户数据持久化

# Best Practice

经典的用法是讲 Nginx和日志读取工具放在同一个 pod 中. 当 nginx 在写日志的时候, 日志读取工具就能读日志. 每一个 Pod 有一个 IP 地址

容器运行时 container runtime 就是正在运行的 docker. K8S 不再支持 docker 的运行时, 表示我们还是可以用 docker compose 文件去别写 docker 的配置. 然后用 containerd 去拉起容器, 形成一个进行时

Node 是计算节点, 可以是物理机, 也可以是虚拟机

Service 有 Service 的 IP 地址, 由 Service的 IP 地址来接受请求, 然后做负载均衡, 发送给 Pod




Istio 是一种 K8S 生态中的一部分

# Anti-Pattern
