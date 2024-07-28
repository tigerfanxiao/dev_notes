# 设备初始化解
1. 初始登录账户 admin, 没有密码

### 授权
先要连接上公网, 配置完管理页面, 才能打授权. 
打好授权后, 需要配置登录密码

# web 页面管理
有两种管理模式,  console 或者 mgmt 口
一般不用 console 口, 而是插Mgmt 口. 

默认情况下的, 管理口 IP 为 `192.168.1.99`
在本端配置 192.168.1.97
然后用浏览器访问 


# 命令

```shell
# 查看所有配置
show full-configuration
# 查询所有接口配置
show system interface 

# 查看指定接口下的配置
show system interface port1
```

### 防火墙配置输出
```shell
# 一次性输出所有配置
config system console
set output standard # 配置输出方式为全部输出
end


# To change it back to the default:  

# 用 more 的方法输出配置, 默认模式
config system console
set output more 
end  

```

用 get 开通是看状态
```shell
# 查看所有接口配置
show system interface

# 修改接口.
edit port6
 set vdom root # 类似于 vrf, 这里默认没有 vrf 的话就是 root
 set type physical
 set snmp-index 6

# 查看单个接口配置
show system interface port1

```


配置命令
```shell
config system interface # 进入接口配置
 edit port1 # 在数字的地方可以 tab
 set mode static # 配置静态 IP 地址, 默认是 DHCP
 set ip 10.1.1.20 255.255.255.0
 set allowaccess ping # 允许被ping. 默认是允许 ping 别人的
 next
end


# 测试直连
execute ping 10.1.1.1
```

注意: 华为设备 ping 别人也要放行. Fortigate 默认可以 ping 别人

打授权一定要能够上公网

```shell
config router static # 配置静态路由
edit 1
set device port4 # 4 号接口
set dst 0.0.0.0 0.0.0.0
set gateway 202.100.1.1
next 
end
# 查看路由表
get router info routing-table all 

# 查看静态路由配置
show router static


```

HA
为什么热备一定要用一套 IP 和 MAC?
因为如果用两套 IP 和 MAC, 在一个设备挂掉的时候, 在交换机中记录的是挂掉的设备的 MAC 地址. 除非启动的另一台冷备设备发送免费的 ARP, 来刷新交换机中的 ARP 表, 否则交换机还是不断的给挂掉的设备发送流量

在传统的技术中, HRSP 就是用虚拟的 IP 和虚拟的 MAC

思科的集群: 所有的请求都会送到 master 设备

Active - Passive 是最稳的, 特性支持最多的. 项目中最主推
虚拟化的墙一般情况下, 特性支持不完整. 一般在网络设计中, 一般没有这种场景
AA 的设计防火墙, 一般 吞吐量和性能只是乘以个数, 再乘以 60%, 一般稳定性也不如 AP

防火墙如果考虑主备, 就必须买两个, 否则后面供应链更不上, 根本配不到一下. 
在思科的防火墙中, 主设备是配置指定的. 主设备必须配置稳, 一个 passive 设备有可能是空配. 设备起来的时候, 主设备给空配设备刷配置. 而不是反向的

在配HA 的时候, 一定要主设备先开机, 在主设备配置完成之后, 才把 passive 可以开机
在配 HA 的时候, 配置优先级, 且配置设备不抢占, 只按照优先级决定主设备

防火墙上有两种接口. Monitor Interface 和 Heartbeat Interface. 
监控口 Monitor Interface 的作用是如果发现接口当了, 立即就切换主备防火墙. 如果不是监控口, 就会不断的重传, 多次超时后才会切换

所以监控接口通常都是处理高优先级网络流量的接口
1. 不要把所有的接口都设置为监控口
2. 不要监控心跳接口
3. 可以监控 VLAN 接口

心跳接口包含敏感的集群配置信息
1. 必须有一个心跳接口, 但为了实现冗余推荐使用 2 个
2. Fortigate 的交换接口, 不能用于心跳接口

思科的设备默认所有的接口都是 monitor interface, Fortigate 需要指定

```shell
# 全局配置
config sys global
set hostname QYT_Active # 名字不会被刷掉
end
```

推荐在页面配置 HA
```shell
show system ha
config system ha
 set group-name gytang # 创建一个组
 set mode a-p
 set password qytangccies
 set hbdev port5 0
 set override disable # 如果要配置抢占 set overide enable 不推荐
 set priority 200
 set session-pickup enabnle # 激活状态化信息同步
end

# 查看 HA 状态
get system ha status
```

一般厂商, 一般是不推荐抢占的. passive 设备连接之前也需要打好授权
passive 设备配置
```shell
config system global
set hostname QYT_Passive
end
config system ha
set group-name qytang
set mode a-p
set password qytangccies
set hbdev port 5 0 # 这里可以定义多个心跳口, 0 表示优先级
set override disable
set priority 100
set session-pickup enable # 激活状态信息同步
```

配置完需要同步 2-3 分钟, 在 web 页面也能看到两台设备

一定要把 Session pickup 打开, 否则状态化信息是没有同步的, 切过去也是全断

自己 ping 自己都行
```shell
# cisco 设备长 ping, 用来验证秒切, 或者 telnet 的 TCP 会话没有断
ping 202.1.1.1 re 9999 
```


Internet Service 互联网服务
基于不同应用走不同的路由, 不同的链路

在 PC 上 tracert 和 traceroute 不是一回事
```shell
# 是用 icmp 做测试的, ttl 超时
tracert -d -w www.baidu.com 

# 思科的 traceroute 是 udp 的包, 发 33434 端口
# 只发一个包(一般为 3 个) 5跳就结束
tracertoute 192.168.50.1 numeric probe 1 ttl 1 5
```

```shell
# 删除所有 dns 缓存
ipconfig /flushdns
```


vyos 虚拟路由器配置方法
```shell
config
set protocols ospf parameters router-id 2.2.2.2
set protocol ospf parameters network 202.100.1.0/24
set protocol ospf parameters network 202.100.2.0/24
commit

```


fortigate 配置ospf
```shell
config system interface
edit port2 
set ip 202.100.2.30 255.255.255.0
set allowaccess ping http ssh https # http 和 https 用来激活网管
config router ospf 
set router-id 3.3.3.3
config area
edit 0.0.0.0 
next
end
config network 
edit 1
set prefix 202.100.2.0 255.255.255.0
set area 0.0.0.0
next
end
end

# 
get router info ospf neighbor
get router info routing-table all 

 
```


```shell

# 看 nat session
get system session list

```


```shell
# 转换端口号做 telnet
telnet 202.100.1.11 2323
```

### Central-nat

需要在命令行开启 Central-nat, 还需要删除一些之前NAT的配置, 除了 DNAT
```shell
set central-nat enable
```


```shell
execute reboot # 重启

```