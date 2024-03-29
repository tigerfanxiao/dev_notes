思科路由器产品线是 8K系列

SASE 包含了 SD-WAN

路由器的口也可以切换成二层口, 叫 bridge, 但是用的不多

```shell
# 在三层交换机上配置把二层口切换成三层
int g0/0/1
no switchport 
```


switchport 两种模式
1. access
2. trunk

服务器的网卡也能支持 trunk

交换机和路由器的口区别
1. 路由器默认是 shut down 状态, 需要打开
2. 交换机默认是插上线是 no shut down

思科的路由器一直可以做防火墙. 
防火墙的定义: 支持状态监控包过滤. 和真正的高级的下一代防火墙还是有些差距, 但是可以做 zone
做站点到站点的 vpn
远程拨号

思科 ASA 的防火墙可以用远程拨号. 
VPN 现在已经是比较老的技术, 现在主要用 SD-WAN 技术
思科的 Catalyst 8K 系列天生就能做 SD-WAN 的客户端, 可以接入思科的 SD-WAN 的解决方案里面去. 

思科路由器是一个网络技术的集大成者. 可以插一个服务器进去. 可以直接把容器运行在思科的服务器上. 

可以插无线的模块. 万一有线网络断了, 可以用 3G, 4G, LTE 做备份

思科的路由器 VPN 比较强
可以做无线控制器. 
几十个人分支, 可能一台思科路由器就解决问题了

NIM 是扩展槽
SM 大槽
On-Board Motherboard 扣板

思科 5K 系列, 其实是一个服务器. 在扩功能时, 不需要硬件查卡了. NFV 网络功能虚拟化

Intel 至强芯片

Catalyst 8Kv 就是真机的虚拟化版本. 


存储的协议 fiber channel 非常的稳, 非常快. 比 TCP 中的 scsi 快 1.8 倍
fiber channel 是那种数据链路层, 链路层, 网络层都在一起, 没有分开. 比较贵


在接口下配置人一个 ip 地址, 一定会在路由表里增加两个条目. 一个条目是完整的 IP 地址, 显示 local
另一个则是该网段的地址, 直连网段. 去往该网段的其他地址, 从这边走. 还会形成一张 ARP 表


在网络调试过程中, 可以开 debug, 但是在现网跑业务的网络中, 不要跑 debug
```shell
debug ip icmp 
```
静态路由
```shell

ip route 10.1.0.0 255.255.0.0 10.1.1.1 # 目的地址, 掩码, 下一跳
```

dns 服务器如果自己不能解析的地址, 就要指定运营商的 dns 地址为转发器


静态路由的问题, 最远的地方可以通, 但是中间的地方不能通. 动态路由协议不会遇到这个问题
必须要保证所有的设备都有所有的路由

NAT 有两种类型
PAT 内部所有用户用同一个地址上网(使用不同的端口)
静态转换 把内部的地址转换为外部的地址. 服务器使用

UDP 的 53 端口是 DNS
UDP 把部分的控制交给应用层. TCP 是在自己本层来控制
UDP 的经典应用场景, 使用的语音和视频. 可以应用于组播和广播. 
TCP 是只能做单播.  IP 层丢包, TCP 会确保数据不丢失
UDP 是允许丢包的. TCP 有重传的机制

TCP 用端口号来标识应用, 源端口号是大于 1023 的随机端口, 目的端口一般是约定的
TCP 经典的三次握手

以下是 TCP 三次握手包的查询方式

```shell
wireshark
tcp.port == 443
tcp.flags.syn == 2 # 2=01 因为 TCP 的 SYN 包的 flag 置位是 01
```


一般三层交换机都不支持NAT

```shell
# 查看 nat 会话映射 137.78.1.100 是目标网段
show ip nat translation  | in 137.78.1.100 
```

一般能买到的域名是二级域名 即 cisco.com在这个基础上可以做三级域名, 比如 `mail.cisco.com`, `www.cisco.com`

```shell
# 路由器连接公网的接口上
int e0/2
ip add 202.100.10.254 255.255.255.0
ip nat outside

# 路由器连接核心交换机的接口上
int g0/0
ip add 10.1.1.254 255.255.255.0
ip nat inside  # 所有的内网流量都会走到这个网关

# 内网有三个网段想要出去
ip access-list standard xiao_pat
permit 10.1.10.0 0.0.0.255 
permit 10.1.20.0 0.0.0.255
permit 10.1.100.0 0.0.0.255

# 从 inside 接口来的原地址可以匹配 xiao_pat 列表的流量, 复用 e0/2 接口的 IP 地址
ip nat inside source list xiao_pat interface e0/2 overload
```

### 静态 NAT 转换
思科叫静态转换. 把内网的 ip 地址映射到可以给外网访问
这个外网地址不一定是配置在接口上的, 但一般是运营商给的公网网段中的. 因为从 ISP 网关可以通过/29 位的直连路由到达客户的网关路由器. 
注意从 ISP 网关到客户网关应用到了代理 ARP 的概念. 因为公网地址没有配置在直连接口上, 所以静态 NAT 配置完成后, 物理接口会代理公网地址告诉 ISP 网关设备自己也是这个公网 IP 地址的对应 mac 地址

如果 ISP 给的是一个非直连网段的公网地址. 则 ISP 需要在自己的网关设备配置到客户网关的静态路由. 把到公网地址的下一跳指向客户网关.

在思科设备上, 这个 NAT 策略是双向的, 就是内网 10.1.100.100 的流量出去转为 202.100.10.100. 如果外网访问 202.100.10.100 则转为内网地址 10.1.100.100
注意: 这个 202.100.10.100 并不是某个接口地址, 而是在公网地址相同的网段上
```shell
int e0/2 # 面向公网的接口
ip add 202.100.10.100 255.255.255.0
ip nat outside

int e0/0 # 内部接口
ip add 10.1.100.100 255.255.255.0
ip nat insde 

# nat 策略
ip nat inside source static 10.1.100.100 202.100.10.100

# 查看 nat session 的记录
show ip nat translation | in 202.100.10.100
```

注意: 国内防火墙厂商, 只管目的, 就是只转换到进来的, 出去的不管



这个静态转换有问题? 还要再看
静态转换比动态转换优先. 也就说在路由器上同时配置了动态 NAT 和静态 NAT 后, 内部服务器访问外网, 会优先使用静态 NAT 策略中配置的公网地址出去, 而不是用动态 NAT 中配置的公网地址出去


