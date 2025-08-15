Simple Network Management Protocol

### SNMP Operation
1. The managed device can notify the NMS of events
2. The NMS can ask the managed devices for information about their current status
3. The NMS can tell the managed devices to change aspects of their configuration 
SNMP 主要用来从被管理设备中获得诸如 IP address, CPU usage, temperature, interface status 这些信息


### SNMP Versions
SNMP 演进了 3 个版本. 其中 SNMP v1 基本用的比较少了, 功能比较有限 

V2c 是用的比较多的版本, c 代表 community 类似password. 主要的问题是没有做加密
V3 是引入了Encryption 和 Authentication 的版本, 只是在配置上有些复杂


### SNMP 设备角色
1. Managed Device 被管理的设备
2. Network Management System (NMS) 管理服务器

NMS 可以从 Managed Device 上获取信息, 也可以配置 Managed Device

### SNMP组件
1. NMS 上 SNMP Application 是人机交互界面. 呈现告警 alert, statistics, charts 
2. SNMP Manager 是服务, 用来和 Managed Device 沟通. request information, 下发配置修改
3. Managed Device 上 SNMP Agent 是客户端, NMS 的 SNMP manager 交互. 每个设备还自己维护一个 MIB Management Information Base 数据库
4. MIB 存储很多参数变量 Variable, 用 OID  Object ID 来表示. 
5. OID 有 Interface Status, traffic throughput, CPU usage, temperature, etc
![[Pasted image 20240322072917.png]]

OID 结构可以通过 oid-info.com 查询
![[Pasted image 20240323083111.png]]
SNMP 通过 community 这个字段来区分是单纯的 get 信息还是配置信息

### SNMP Message
1. GET, GetNext, GetBulk 是 NMS 向 Managed Device 发的. 
2. Trap, Inform 是 Managed Device 想 NMS 发的. 
3. Trap 和 Inform 的区别在于, 虽然都是基于 UDP, 但是 Inform 是需要等待 NMS 确认收到发送 Response, 否则会重发
![[Pasted image 20240323083931.png]]

### SNMP Port
SNMP Agent = UDP 161 
> NMS 把 set, get 信息发送到客户机的这个端口

SNMP Manager = UDP 162 
> 客户机的 trap, inform 消息发送到 NMS 的这个端口

### SNMP Package

以下是客户机发送给 NMS 的 trap 报文
![[Pasted image 20240323085527.png]]
### SNMP 和 Syslog 的区别
1. SNMP 可以主动从被管理设备获取信息和配置信息 get, set. 但是 syslog 是设备向日志服务器去写信息
2. SNMP 被管理设备的配置, 一般不需要指定管理设备的具体地址, 可以给一个 acl, 允许某个网段的管理设备来访问. 



华为设备
```shell
# 因为密码太容易了, 所以需要关闭
snmp-agent community complexity-check disable

acl number 2099
rule 5 permit source 192.168.0.0 0.0.255.255

# cnmonitor 是密码
snmp-agent
snmp-agent community read cipher cnmonitor acl 2099 alias cmn
# 把 system info 都能读取吧
snmp-agent sys-info version v2c v3
```

思科设备
在客户机上指向 SNMP 服务器
```shell
# 可选的
snmp-server contact xiao@fan.com
snmp-server location xiao-home

# 如果不定义 community 默认ro 是 public, rw 是 private

snmp-server community xiao1 ro # 只读
snmp-server community xiao2 rw # 读写
snmp-server host 192.168.1.1 version 2c xiao1 # 只能读

# 客户设备向服务器发送 linkdown 和 linkup 信息
snmp-server enable traps snmp linkdown linkup 
snmp-server enable enable traps config 

```

