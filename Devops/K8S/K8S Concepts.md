# K8S Architecture

- 至少有一个 Master Node, 和多个 worker node
- Master Node 输入 control plane 也称为管理平面
- Node 可以理解是一个物理机, 或者一台虚拟机
- 每一个 worker node 上都有Kubelet(K8S Process), 用于不同的 cluster 之间的通信
- 每个worker node 上都有多个 Pod, Pod 是 K8S 最小的管理单元. 一个 Pod 中可以有多个容器. 一个 Pod 中的多个容器可以共享同一片存储. 

### Master Node Process
- API Server: Entrypoint to K8S cluster
- Controller Manager: Keeps track of what happening in the cluster
# Best Practice

经典的用法是讲 Nginx和日志读取工具放在同一个 pod 中. 当 nginx 在写日志的时候, 日志读取工具就能读日志. 每一个 Pod 有一个 IP 地址

容器运行时 container runtime 就是正在运行的 docker. K8S 不再支持 docker 的运行时, 表示我们还是可以用 docker compose 文件去别写 docker 的配置. 然后用 containerd 去拉起容器, 形成一个进行时

Node 是计算节点, 可以是物理机, 也可以是虚拟机

Service 有 Service 的 IP 地址, 由 Service的 IP 地址来接受请求, 然后做负载均衡, 发送给 Pod


Istio 是一种 K8S 生态中的一部分

# Anti-Pattern
