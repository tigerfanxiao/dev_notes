
centos 或者 redhat 使用 yum 来安装软件. 安装软件有多种方式. 
1. 可以直接下载二进制包, 如果执行 bash 脚本来安装
2. 可以下载代码, 在本地编译后安装
3. 可以把下载二进制包的网址添加到 yum 的远程仓库中, 自动下载安装


下面我们介绍第 3 种方法

```shell

# 在 CentOS 8 中安装 Terraform
# 先确认一下 yum-utils 安装工具有没有
sudo yum install -y yum-utils

# 把仓库的地址添加到 yum-config-manager 中
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
# 下面是添加成功的回显
Adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
# 安装 terraform
sudo yum install -y terraform

```


