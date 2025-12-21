安装在 ubuntu 22.04 
首先关闭ubuntu 22.04 上面你的 cloud-init 否则不能配置静态IP地址
Key point: Disabling cloud-init network prevents it from touching Netplan in the future, 
but it does not automatically delete the old 50-cloud-init.yaml. You must remove it manually and create your own netplan file.

# Disable Cloud-init
Create a file which is used to diable cloud-init
```shell
sudo vim /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```
文件内容如下
```yaml
network:
  config: disabled

```
- remove old cloud-init config
```shell
sudo rm /etc/netplan/50-cloud-init.yaml
```

- add new netplan config
```shell
sudo vim /etc/netplan/01-netcfg.yaml
```

文件内容如下
```yaml
network:
  version: 2
  ethernets:
    ens33:
	  dhcp4: no
	  addresses:
	    - 11.0.1.129/24
	  routes:
	    - to: default
		  via: 11.0.1.2
	  nameservers:
	    addresses:
		  - 8.8.8.8

```
重启网络服务
```shell
sudo netplan try
sudo netplan apply

sudo reboot
# should return disabled or done
cloud-init status
```

- edit host file
```shell
sudo vim /etc/hosts
```

增加如下内容
```shell
# add 
11.0.1.129 master
11.0.1.130 node1
11.0.1.131 node2
```

- Disable swap
```shell
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

验证swap被关闭
```shell
free -h
xiao@node1:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           3.8Gi       306Mi       3.2Gi       1.0Mi       256Mi       3.3Gi
Swap:             0B          0B          0B
```

# lood local kernal moduls
```shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

# set sysctl parameters
```shell
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
sysctl net.ipv4.ip_forward
```
# install containerd
```shell
sudo apt update
sudo apt install -y containerd
```


# generate config
```shell
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

- change /etc/containerd/config.toml 中的 SystemdCgroup = true
# restart containerd
```shell
sudo systemctl restart containerd
sudo systemctl enable containerd
sudo systemctl status containerd
containerd -v
```


# Install k8s package
```shell
sudo apt install -y apt-transport-https ca-certificates curl
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key \
  | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" \
| sudo tee /etc/apt/sources.list.d/kubernetes.list
```
# install kubeadmin/kubelet/kubectl
```shell
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
```

验证kubeadm kubectl安装完成
```shell
kubeadm version
kubectl version --client
```


Master node only, install control plan
必须在control plan的master节点运行
```shell
sudo kubeadm init --kubernetes-version=v1.29.0 \
  --pod-network-cidr=192.168.0.0/16
```
Save the kubeadm join command printed at the end.

# 这里非常重要, 必须要root运行 kubeadm join
```shell
# 在master节点上运行, 获得join 命令
kubeadm token create --print-join-command
# 在worker节点上运行, 必须要用sudo
sudo kubeadm join 11.0.1.129:6443 --token 9jw4pe.9vcoy425t7vqnl7b --discovery-token-ca-cert-hash sha256:cb5ce084f69c55ff48759b68583a8026247b3ea02ee8d7c94f4d9d420722bd20
```


这个在master 在 kubeadm init 之后运行, 在worker node 在 kubeadmn join之后运行
On Ubuntu 22.04, containerd socket moved from /var/run → /run.
Kubeadm defaults may still point to /var/run/containerd/containerd.sock.
Without this fix, the worker cannot start kubelet properly.

```shell
sudo sed -i 's|/var/run/containerd/containerd.sock|/run/containerd/containerd.sock|' /var/lib/kubelet/kubeadm-flags.env
sudo systemctl daemon-reload
sudo systemctl restart containerd
sudo systemctl restart kubelet
```


config kubectl access 只在master节点做
```shell
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

- 验证master节点安装成功
```shell
xiao@master:~$ kubectl get nodes
NAME     STATUS   ROLES           AGE     VERSION
master   Ready    control-plane   3m31s   v1.29.15
```

# Install Calico
```shell
kubectl apply -f \
https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/calico.yaml
# 验证
xiao@master:~$ kubectl get pods -n kube-system
NAME                                       READY   STATUS              RESTARTS   AGE
calico-kube-controllers-5fc7d6cf67-fklmv   0/1     ContainerCreating   0          33s
calico-node-q6msx                          0/1     Running             0          33s
coredns-76f75df574-ccsl8                   1/1     Running             0          2m43s
coredns-76f75df574-xvgkv                   1/1     Running             0          2m43s
etcd-master                                1/1     Running             0          2m57s
kube-apiserver-master                      1/1     Running             0          2m57s
kube-controller-manager-master             1/1     Running             0          2m57s
kube-proxy-cnqf8                           1/1     Running             0          2m43s
kube-scheduler-master                      1/1     Running             0          2m57s

```


