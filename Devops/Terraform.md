
[ B 站参考视频](https://www.bilibili.com/video/BV1L34y1B7PT/?spm_id_from=333.880.my_history.page.click&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)

# What is Terraform 
* Terraform is  a tool for creating and configurating infrastructure
	现实中, 部署基础设施(provisioning infrastructure)和在基础设施上部署应用(deploying application), 是两个步骤. Terraform 一般使用在云计算场景下的或者虚拟机场景下的基础设施构建
* Univeral IaC tool 用同一个工具可以配置不同的平台, 比如混合云
* Open source 
* Declarative method

## Declarative vs Imperative

Declarative所谓声明式的, 就是说我声明我要的最终结果, 工具会比较现在的状态, 和 desired 的状态, 自己完成配置的修改. 
Imperative 命令式, 就是我必须告诉设备每一步修改的动作, 以至实现我最终期望的状态. 
举个例子. 我现在有 4 台虚机 如果我用希望把虚机的个数增加到 7 台. 用声明式的写法就是, 最终 7 台虚机. 用命令式的写法就是 - 增加 3 台虚机

## Terraform vs Ansible
本质上两者都是 IaC 工具. 
Terraform 主要是构建基础设施, 从无到有. 而 Ansible 是用来配置和修改基础设施. (基础设施已经构建好了)

## Terraform的使用场景
1. 对已有的基础设施, 做扩充
2. 重构基础设施, 用来做实验. 实验环境和生产环境用同样的基础设施

## Terraform的架构
有两个组件: 一个是Terraform核心， 一个是Provider用来连接AWS/Azure/K8S
1. Core 核心, core 有两个输入. 分别代表着当前的状态和期望的状态.
	1. TF-Config  定了我们需要部署什么, 代表期望的状态 end state
	2. State 当前状态， 通过query provider来实现
	Core 会比较最终状态和当前状态, 构建 execution plan. 这个 Plan 里面包含了,哪些资源要被创建, 修改和销毁.


# 安装 Terraform

```shell

# 在 CentOS 8 中安装 Terraform
# 先确认一下 yum-utils 安装工具有没有
sudo yum install -y yum-utils

# 把仓库的地址添加到 yum-config-manager 中
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
# 下面是添加成功的回显
# Adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo

# 安装 terraform
sudo yum install -y terraform

terraform -help  # 测试 terraform 安装成功

touch ~/.bashrc
terraform -install-autocomplete  # enable tab 提示, 本质上是在 bashrc 中加一行

```

在 mac 上
```shell

# 假设 homebrew已经安装好了
brew upgrade # 先更新已有的 package
brew install terraform # 安装

touch ~/.bashrc
terraform -install-autocomplete

```
在 windows 上使用choco 作为包管理工具来安装
这里省略了


# Terraform命令

refresh 查询provider得到state
plan 创建一个execution plan, preview what will happen
Apply 执行 execution plan
destroy 销毁所有创建的基础设施

