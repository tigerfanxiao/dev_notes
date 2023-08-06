### 参考资料
[思科模块](https://docs.ansible.com/ansible/latest/collections/cisco/ios/ios_config_module.html)

### Ansible 相关产品
我们一般说的 Ansible 只是一个执行器. 在这个执行器之上, 如果要实现 RBAC 和 LDAP 两种协议. 就开发了 Ansible AWX 及其商用版本 Ansibe Power
RBAC 即 role-based access control  对不同的角色分配不同的权限
LDAP 即 Lightweight Directory Access Protocol 对用户身份进行验证. 


# 概念

### Ansible 组件

1. Ansible 是不需要在设备上装什么 agent 的. 
2. Ansiblie 通过 module 向设备发送命令. 这个 module 是不同的设备厂商用 python 开发的. 类似与驱动. 每个 module 有些特殊的用法. 里面使用到的参数要在 playbook 里写. 比如发送的命令

### Ansible 核心文件
Ansible 要运行, 需要一下文件
1. `ansible.cfg`  文件
2. `inventory` 文件, 用于配置目标网元和变量
3. playbook用 yaml 写的, 可以基于 jinja2 来渲染


### Ansible 配置文件
每个任务都可以配置一个独立的 `ansible.cfg` 配置文件
1. 当前文件夹. 可以为每一个项目创建一个文件夹
2. 在 home 目录下
3. `/etc/ansible/ansible.cfg`

### Ansible Inventory 文件
inventory 文件可以支持多种格式. 在 ansible 上可以配置不同的插件来读取不同格式的 inventory 文件. 
inventory 文件定义的地方
1. `/etc/ansible/hosts`
2. 环境变量 ANSIBLE_INVENTORY
3.  ansible.cfg中指定的地方


默认所有的设备都属于 all 群组

ini 格式的 inventory 文件样例
``` shell
csr1kv4  # 单独一台设备 可以被单独执行. 在执行 all 群组命令时也会被执行
[datacenters:children]  # 群datacenters, children 表示群下面定义了 2 个群
dc_east  # 群 dc_east
dc_west  # 群 dc_west

[dc_east]
10.10.10.1
csr1kv1

[dc_west]
csr1kv2
switch.cisco.com
```

带有变量的 inventory
```
[cpe] # 定义了变量
csr1kv1 loopback_id=1 location=SF # 给群里的单个 host 定义变量
csr1kv3 loopback_id=3 location=LA

[core]
csr1kv2 loopback_id=2

[usa:children] # usa 群下面有 children, children 下面有两个群 cpe 和 core
cpe # 
core

[all:vars]  # 这里定义变量
ansible_ssh_pass=cisco
ansible_user=cisco
ansible_network_os=ios

[cpe:vars]  # 给 cpe 订货易的变量
location=California

[core:vars]
location=NVC

```

yaml 格式的 inventory文件样例
```yaml
---
all:
  hosts:
  - csr1kv4
datacenters :
  children:
  - dc_east
  - dc_west
routers:
  hosts:
  - csr1kv2
  - switch.cisco.com
switches:
  hosts:
  - 10.10.10.1
  - csr1kv1

```

目录结构
变量文件
群
```
.
├── group_vars
│   ├── ios
│   │   ├── snmp.yml
│   │   └── interfaces.yml
│   └── nxos
│       ├── snmp.yml
│       └── interfaces.yml
├── inventory
└── playbook.yml
```

独立 host
```
.
├── host_vars
│   ├── csr1kv1
│   │   ├── snmp.yml
│   │   └── interfaces.yml
│   └── nxos-spine1
│       ├── snmp.yml
│       └── interfaces.yml
├── inventory
└── playbook.yml
```

### Ansible Playbook
Playbook 一般是有 YAML 文件. YAML 对空格是严格要求的. 

Playbook 中的元素
1. **Play name:** Play name is what you will call the play. It is also an arbitrary description, an option of the play, and it is displayed to the terminal when executed.
    
2. **Hosts:** Hosts indicate which devices Ansible operates for the play. The inventory file has a group that is called iosxe, which can have IP addresses or fully qualified domain names (FQDNs) of the hosts.
    
3. **Connection:** The network_cli is a connection plug-in that provides a persistent connection to remote devices over SSH.
    
4. **Gather facts:** Ansible collects device facts by default. Since you execute modules locally, there is no need to gather facts.
    
5. **Tasks:** Each task has the following attributes.

```yaml
---
- name: playbook name
  hosts: iox-xe # 在 inventory 文件中定义的组或者单台设备, ip 地址, 或者 hostname
  connection: network_cli # Ansible使用的连接设备的模块
  gather_facts: no  # 是否保存设备的 meta 信息

  tasks: # 任务列表
    - name: TASK ONE
      MODULE_NAME:
	        key-1: value1
	- name: TASK TWO
	  ios_config:
	    commands:  # command 和 是 list 的别名
	    # 这里 src, commands 是互斥的. src 放配置的源文件  src: cisco_ios.cfg
	      - snmp-server community public RO
	      - snmp-server community private RW
```

变量文件

```
[all:vars]
ansible_ssh_pass=cisco
ansible_user=cisco
ansible_network_os=ios

snmp_community=cisco_corse
snmp_location=CA_HQ
snmp_contact=JOHN_SMITH

[ios]
csr1kv1  node_id=1
csr1kv2  node_id=2
csr1kv3  node_id=3
```


### Ad-Hoc Command
`-v` 表示 verbose

```shell
ansible-playbook -i inventory playbook_name.yml -v 
```


# 案例

playbook
```yaml
---

- name: MAKE CONFIG CHANGES USING HOST_VARS AND GROUP_VARS
  hosts: all
  connection: network_cli
  gather_facts: no

  tasks:

    - name: CONFIGURE HOSTNAME USING VARIABLES STORED IN HOST_VARS
      ios_config:
        commands: "hostname nycr-{{ node_id }}"
```

变量
文件
```
[all:vars]
ansible_ssh_pass=cisco
ansible_user=cisco
ansible_network_os=ios

snmp_community=cisco_corse
snmp_location=CA_HQ
snmp_contact=JOHN_SMITH

[ios]
csr1kv1  node_id=1
csr1kv2  node_id=2
csr1kv3  node_id=3
```


### Ansible Galaxy
Ansible Galaxy类似于 python 中的 PIP, 有页面. 可以通过 Ansible Galaxy 命令来安装第三方的模块
```shell
ansible-galaxy install ansible-netowrk.cisco_ios 
```

### Ansible doc

当无法访问外网时, 可以通过 Ansible doc 命令来查看已经安装的模块的参数
cisco的模型不只是 ios_config 还有 iosxr等

```shell
ansible-doc ios_config
```