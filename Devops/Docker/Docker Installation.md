# 安装docker

### centos

```shell
sudo yum remove -y docker \\
                  docker-client \\
                  docker-client-latest \\
                  docker-common \\
                  docker-latest \\
                  docker-latest-logrotate \\
                  docker-logrotate \\
                  docker-engine


sudo yum install -y yum-utils, device-mapper-persistent-data, lvm2
# 添加仓库
sudo yum-config-manager --add-rep https://download.docker.com/linux/centos/docker-ce.repo 
# 安装
sudo yum install -y docker
# 开启 docker 服务, 启用 docker
sudo systemctl start docker && sudo systemctl enable docker
# 把cloud_user放到 docker 组里, 以后运行 docker 命令, 不需要 sudo
# 要使下面命令生效, 需要推出 shell
sudo usermod -aG docker cloud_user 
```


### Ubuntu

Remove existing Docker installs. update the ubuntu

```shell
sudo su
sudo apt update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository  "deb [arch=amd64] <https://download.docker.com/linux/ubuntu> $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod cloud_user -aG docker
```


docker 安装完成后

```shell
# 启动 docker
systemctl start docker

# 激活这个服务, 每次开机都会启动
systemctl enable docker

# 查看 docker 的版本
systemctl status docker
docker version 


```

### 配置阿里云镜像加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json << -'EOF'
{
	"registry-mirrors": ["https://0a041wc3.mirror.aliyuncs.com"]
}
EOF
# 重启docker的守护进程
sudo systemctl daemon-reload 
sudo systemctl restart docker

```