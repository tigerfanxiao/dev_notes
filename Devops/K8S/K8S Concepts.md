Orchestration 的作用

- High availability with no downtime
- Scalability or high performance
- Disaster Recovery for backup and restore

### K8s Component and Objects

- node 每一个物理机或者实体机可以看做为一个 node
  - 至少有还一个管理节点 Master node, 1 个或者多个 worker node

- ConfigMap 保存 pod 的配置信息. 作用是当 pod 的配置信息变动时, 我们不需要重新 build image, 只需要修改 configmap 就行. 比如 App pod 需要交互的数据库 pod 的 url, 有点类似保存 pod 使用的环境变量
- Secret 用于向 pod 传递密码信息. 比如如果 App pod 连接数据库的账号密码变动了, 明文放在 Config-map 中就很危险, 所以放在 secret 中
- Deployment 通过 template 来规定 pod 创建完成的状态(包括 replica 的数量), 是 Pod 的 blueprint . 我们平时不直接操作 Pod 而是配置 Deployment. 比如当一个 node 节点挂了, 依据 Deployment 的配置里如果说有 2 个 node, 那 K8s 把流量转给 replica, 然后就会自动重构一个 pod, 实现 deployment 中定义的架构. Deployment 适用于定义 stateless 的 app

- K8s 有 control plane, 也称为 master node, 工作节点称为 worker node 在控制平面上有 API Server, Scheduler, Controller Manager, etcd
  - API Server 接受各种对容器的控制指令
  - Scheduler 已经 node 的资源占用, 选在在不同 node 上创建 pod
  - Controller manager 探查 Cluster 的状态变化, 比如哪个 pod 挂了, 就通知 Scheduler 调用 node 上的 kubelet 重启 pod
  - etcd is key-value store, holds the current status of k8s component. etcd 在在主备的 master node 上可以做成分布式存储.
  - 最佳实践是 master node 更重要, 所以需要冗余, 但是需要的资源对比 worker node 要少
- Kubectl k8s 管理工具, 用来创建 k8s 各种 component
- kubeadm k8s 管理工具, 用于管理 kubernetes 版本升级, 创建集群 cluster
- API Server: Entrypoint to K8S cluster 管理员可以通过 UI, API 或者 命令行 Kubectl 来访问 K8S
- Scheduling k8s 把 pods 分配到不同 node 的过程
  - Scheduler 在控制平面确保 pod 的 provision 在哪个 Node 上, 每个 pod 使用多少资源
- RBAC Role based access account
  - ServiceAccount: 每个 pod 默认都会有一个 service account 与 k8s 的 api 进行交互
- ReplicaSet

# 安装 K8s

### 基本要求

- 至少 3 个 node, 其中一个 master node, 2 个 worker node
- 每个 node 至少 2 个 cpu 和 2g ram
  一般情况下, regular node 是通过 api server -> Scheduler -> Kubelet 来构建的. 但是 master node 所在的 control plane 是直接通过 kubelet 来构建的. Kubelet 可以观察 `/etc/kubernetes/manifests` 目录下的查找 manifest file 的来构建 static node
- Kubelet (Not Controller Manager) watches static pod and restart if it fails
- Static Pod names are suffixed with node name
- Static Pod 组成: API server, scheduller, controller manager, etcd

# Best Practice

经典的用法是讲 Nginx 和日志读取工具放在同一个 pod 中. 当 nginx 在写日志的时候, 日志读取工具就能读日志. 每一个 Pod 有一个 IP 地址

容器运行时 container runtime 就是正在运行的 docker. K8S 不再支持 docker 的运行时, 表示我们还是可以用 docker compose 文件去别写 docker 的配置. 然后用 containerd 去拉起容器, 形成一个进行时

Node 是计算节点, 可以是物理机, 也可以是虚拟机

运作机制

- 使用 self-signed CA certificate for K8s
- Server certificate for the API server endpoint
- Client certificate for Scheduler and Controller Manager
- Server certificate for Etcd and Kubelet
- Client certificate for API server to talk to Kubelets and Etcd
- Client certificate for Kubelet to authenticate to API server
  Kubeadm
- Providing "fast paths" for creating k8s cluster
- perform the actions necessary to get minimum viable cluster
- It cares only about bootstrapping, not about provisioning machines

### Istio

# k8s Management Tools

Kompose 把 docker compose 文件变成 k8s 对象
Helm k8s 配置工具 template
Kustomize k8s 配置工具 template
