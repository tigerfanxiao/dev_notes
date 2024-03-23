SNMP 主要用来从被管理设备中获得诸如 IP address, CPU usage, temperature, interface status 这些信息

SNMP 演进了 3 个版本. 其中 SNMP v1 基本用的比较少了, 功能比较有限 

V2c 是用的比较多的版本, 主要的问题是没有做加密
V3 是引入了加密的版本, 只是在配置上有些复杂


### SNMP 设备角色
1. Managed Device 被管理的设备
2. Network Management System (NMS) 管理服务器

NMS 可以从 Managed Device 上获取信息, 也可以配置 Managed Device

### SNMP组件
1. NMS 上 SNMP Application 是人机交互界面. 
2. SNMP Manager 是服务, 用来和 Managed Device 沟通
Managed Device 上 SNMP Agent 是客户端, 每个设备还自己维护一个 MIB 数据库
![[Pasted image 20240322072917.png]]


SNMP 通过 community 这个字段来区分是单纯的 get 信息还是配置信息

SNMP 和 Syslog 的区别
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

```shell


```