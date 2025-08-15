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
  connection: network_cli # Ansible core 使用的SSH连接设备的模块
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
`-i` 表示 inventory 文件
`--check` 表示 dry mode

```shell
ansible-playbook -i inventory playbook_name.yml -v 
```


# 案例

ios_config 参数
`before` 只有在变化的时候, 才会在 commands 执行前运行. 一般和 `match: exact` 连用. 保证 commands 中的内容和顺序不变
`match` 与 `before` 连用
`replace: block` 可以替代`match` 与 before 连用. 表示 `before` 执行完之后, `commands` 整理输出

```yaml
---
  - name: MANAGE ACLS
    hosts: all
    connection: network_cli
    gather_facts: no

    tasks:

      - name: Configure ACL on IOSXE 
        ios_config: 
          parents: ip access-list extended INBOUND 
          commands: 
            - permit ip host 172.16.2.2 any log 
            - permit ip host 172.16.1.1 any log 
            - permit ip host 172.16.3.3 any log 
            - permit ip host 172.16.4.4 any log 
            - permit ip host 172.16.5.5 any log 
            - permit ip host 172.16.6.6 any log
          before: no ip access-list extended INBOUND 
          replace: block
          # match: exact
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


## Anisble自有模块

- debug
	- var
	- msg
- register

- set_fact
	- table 
## 思科 Ansible 模块
- ios_config
- ios_facts 用来采集设备信息
- ios_command 里面可以直接下发 show 命令

### ios_facts 组件
专门用来采集用了ios系统的思科设备。他的所有 key值都是下面形式 `ansible_net_<fact>`
- 使用dubug关键词可以用来打印回显的json对象
- 使用register关键词可以用来把回显的json对象保存在某个变量里

```yaml
---

  - name: GATHER FACTS FROM CISCO DEVICES
    hosts: all
    connection: network_cli
    gather_facts: no

    tasks:
      - name: GATHER ALL FACTS
        ios_facts: # 使用ios_facts组件，采集设备信息
        gather_subset: all  # 默认值是 !config
        
	  - name: Gather everything except for hardware
	    ios_facts:
		    gather_subset: 
		      - "!hardware"  # 除了hardware的信息
		register: not_hardware

	  - name: VIEW CONFIG
	    debug: 
	      var: not_hardware # 打印 not_hardware

	  - name: Only gather interface
	    ios_facts:
	    gather_subset: "interfaces"

      - name: ACCESS FACTS VARIABLES
        debug:
          var: ansible_net_version  # 这里的变量名是iso_facts中定义好的变量名

```

### ios_command 模块

debug可用的参数有 var 和 msg
- msg 用于输出定制化的字符串
- var 只用于输出定义好的变量

```yaml
---

  - name: OPERATIONAL COMMANS ON CISCO
    hosts: all
    connection: network_cli
    gather_facts: no

    tasks:

      - name: SEND A SINGLE COMMAND
        ios_command:
          commands: show version # 可以放列表和单个元素
        register: output  # 把结果保存在 output变量中

      - name: VIEW FIRST ELEMENT OF A LIST
        debug:
          var: output['stdout'][0]  # 变量的结果在 stdout 键下的第一个元素

      - name: SEND LIST OF COMMANDS
        ios_command:
          commands:  # 可以放列表
            - show version
            - show ip interface brief
        register: output

      - name: VIEW SECOND ELEMENT OF A LIST
        debug: 
          var: output['stdout'][1]  # 这是第二个元素

	  - name: STORE DATA IN A FILE
	    copy: 
	      content: "{{ output['stdout'][1] }}"
	      dest: ./outputs/{{ inventory_hostname }}.txt  # 这里 inventory_hostname 是默认的变量. 有多少设备就会写多少个文件
	  
	  - name: CREATE TABLE
        set_fact:  # 创建表格
          table: |

                  +----------+--------------+
                  |  HOST    |  {{ inventory_hostname }}     |
                  +----------+--------------+
                  | VERSION  | {{ ansible_net_version }}     |
                  +----------+--------------+
                  | SERIAL#  | {{ ansible_net_serialnum }}  |
                  +----------+--------------+
                  | MODEL    | {{ ansible_net_model }}     |
                  +----------+--------------+

      - name: VIEW TABLE
        debug:
          msg: "{{ table.split('\n') }}"  # 这里支持 python 语法
	      
	      
```


### Ansible辅助工具

td4a 是一个渲染 ansible 模板的工具, 且是开源的, 值得研究
https://github.com/cidrblock/td4a/