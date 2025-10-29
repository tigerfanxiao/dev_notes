# 文档

https://docs.fortinet.com/product/fortigate/6.4

# 概念

下一代防火墙很重要的概念是引入了 session table 的概念
进过防火墙的流量都会在 session table 中创建一条记录

### Session Table 的影响
- 第一次查询了 FIB 后, 往返的流量就会在 sessiontable 中创建一条记录
- 正因为有了 session table 所在 HA 的 AP 切换时, 除了同步配置, 还需要同步 session table
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
### 修改管理页面端口
将 HTTP 端口 80重定向到 HTTPS 并将 8443 作为端口
![[Pasted Graphic 4.png]]


# 命令

```shell
# 查看所有配置
show full-configuration
# 查询所有接口配置
show system interface 
# 查看指定接口下的配置
show system interface port1
# 查看静态路由配置
show router static 
# 查看 FIB
get router info kernel 
# 查看 RIB
get router info routing-table all
```

打授权的网络准备
```shell
# 设置设备名字
config system global
set hostname FW-Active
end

# 配置带内管理, 这样 PC 可以用来做网页管理和配置
# 配置完成后使用 PC 浏览器 打开 https://10.1.1.10
# 第一次登录账号 admin 密码为空
config system interface
edit port1
	set mode static
	set ip 10.1.1.10 255.255.255.0
	set allowaccess ping https ssh http fgfm # 注意: 治理的 ping 是允许别人 ping 我们
next
end


# 打授权需要配置公网 IP 和默认路由
# 配置公网出口 IP 地址
config system interface
edit port4
set mode static
set ip 202.100.1.10 255.255.255.0
set allow ping https ssh http # 运行别人来 ping 我
next
end

# 配置默认路由
config static route
edit 1 
	set dst 0.0.0.0 0.0.0.0 
	set gateway 202.100.1.1 # 公网网关
next
end

# 查看路由表
get router info routing-table all

```

ping 命令
```shell
execute ping 10.1.1.10 # 我 ping 别人
```

# Zone

有的防火墙用 zone, 有的防火墙用安全级别
Fortigate 是 zone 防火墙, 但不是必须的

在 Interface 的地方创建 zone. zone 也会直接显示在 interface 的页面里. 而且创建了 Zone, 接口就会从 Interface 的列表中移出来

Block Intra-zone traffic 默认选择. 表示默认 zone 内也需要做策略来管理. 如果多个接口在一个 zone 里面, 这两个接口是不通的





# HA
传统有 3 种模式
在现实工作中 AP 是用得最多的, 也是最稳的. 特性支持最完整的

## Active-Passive Cluster
- 两个设备, 一个主, 一个备. 
- 主和备都要上电. 各自都需要配置一个 IP 地址, 同时生成一个虚拟的 IP 地址. 
- 谁是主用的, 就用那个虚拟的 IP 和 MAC. 使用 HRSP 协议

### 配置方法
需要配置两个接口, 一个是心跳接口, 一个是监控接口
心跳接口
- 必须要有一个心跳接口. 为实现冗余, 建议使用 2 个
- Fortigate 交换接口不能用于心跳接口
监控接口
- 监控接口一般是用于处理高优先级流量的网络接口. 只要这个接口 down 了, 主备就会切换
- 不要监控心跳接口
- 可以监控 VLAN 接口
注意: 如果不是监控口, 会在多次超时后, 才会切换

主设备配置方法
```shell
# Active 设备配置方法
config system interface
	edit mgmt # 本地 primary 的口
		set ip 30.115.39.67/26
	next
end

config system ha
	set group-name "HA-group"
	set mode a-p
	set hbdev "dmz" 200 "ha1" 100 # 设置两个心跳口, 且有优先级 
	## 如果做一个心跳口
	## set hbdev "port5" 0
	set session-pickup enable # 主备切换时, 主的状态信息同步给备机
	set ha-mgmt-status enable
	
	config ha-mgmt-interface
		edit 1
			set interface "mgmt" # 在 primary 设备上的物理接口
			set gateway 30.115.39.65 # 这是一个虚拟地址
		next
	end
	set override disable # 拒绝被强占, 被刷空配
	## 放开使用强占
	## set override enable
	set priority 200 # 设置主设备优先级
	set ha-direct enable
	set monitor "port1" "port2" "port3" "port4"
end
```
备用设备配置方法
```shell
config system global
set hostname FW-Passive
end

config system ha
	set group-name "HA-group"
	set mode a-p
	set hbdev "dmz" 200
	set override disable # 拒绝被强占, 被刷空配
	set priority 100 # 设置备用设备优先级为 100
	set ha-direct enable
	set monitor "port1" "port2" "port3" "port4"

# 查看 HA 状态
get system ha status


```

## Active-Active Cluster
- 依然会有主备. 主用设备拿着虚拟的 IP 和 Mac
- 所有的流量都会给到主要设备 , 然后主设备本身会控制负载分担
- 但是这种负载分担, 其实有比较大的消耗. 性能不是倍增的. 稳定性不如 AP

注意点: 
做 HA的时候, 可能存在空配设备把有配置的设备刷没了. 注意配置保存. 可以强行要求, 优先级高为 primary 设备. 因为设备被刷, 会被托管

### 测试 HA
telnet 到一台设备, 自己 ping 自己
- Telnet 表示 TCP 会话没有断开
- Ping 查看是不是秒切


# IP Policy

# Routes

### Static Routes
- 可以配置管理距离
![[Pasted image 20240728180647.png]]

### SD-WAN
本质上是公网出口的负载均衡器. 需要把公网接口从zone 中间剥离出来. 创建 SD-WAN 组, 并在静态路由中指定下一跳为 SD-WAN 组

### Route Policy
可以从源目的 IP 和端口号颗粒度来控制路由

### Internet Service
可以对 AWS 等流量, 指定路由策略, 从哪个出口出去
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