
微软服务器
在 win 服务器版本中, 打开服务器管理器, 选择角色, 添加角色, 装 web 服务器 IIS. IIS 既可以当 http 服务器, 也可以做 FTP 服务器

ftp 也可以创建

```shell
# 在 windows 上访问 ftp 服务器
ftp 10.1.0.100
# 匿名
anonymous
# 密码
随便输入邮件
dir # 列出目录
get filename.txt # 下载
bye # 离开

notepad filename.txt
```

交换机的接口默认是二层口, 默认是打开的
三层接口,或者路由器的接口, 默认是关闭的. 

vlan 间通信. 在同一个交换机内. 
每一个网段, 每一个 vlan, 都需要一个出口, 这个出口就是一个 IP 地址, 这个出口就是网关. 
如果你要从一个vlan一个出口访问另一个 vlan 的出口, 就需要通过网关经过三层设备来通信, 一般是坐在核心交换机上

网关可以是路由器的接口, 也可以是一个虚拟的接口. 思科在交换机上的网关叫 svi, 习惯性用.254 结尾. 也有用.1 的

网关, 除了去往直连设备, 就是走网关
```shell
# 激活三层交换机的路由功能
ip routing

# 创建 vlan 接口
int vlan 10
ip add 10.1.10.254 255.255.255.0
no sh


# 对物理接口把二层接口切换成三成接口
int f0/1
no switchport 
```

单臂路由现在很多情况下用在防火墙上

重要: ARP 表只能显示同一个广播域, 同一个 vlan 中的设备 ip 和 mac 地址之间的映射. 如果是跨 vlan 通信, 则只会显示网关的 IP 地址和 mac 地址映射

 PC 没有路由转发能力, 一般就是显示默认网关和直连路由
 PC 需要做网关的 arp 解析
```shell
# 在 pc 上查询路由
route print
```

交换机开启三层功能后, 就有 ARP 表了
```shell
show arp # 查看交换机上的 arp
```

三层交换机工作原理
1. 先看路由
2. 在看 arp
3. 再看 mac 地址表


DHCP
让普通用户来配置 IP 地址不太科学
能够动态为客户配置 
1. IP
2. 掩码
3. 网关
4. dns
网络设备, window server, linux 都可以扮演 DHCP server. 

DHCP 四次握手流程
 1. DHCP Discover 发现网络的 DHCP 服务器 (广播)
 2. DHCP Offer 网络可能存在多个 DHCP 服务器, 客户端可能收到多个 Offer
 3. DHCP Request 告诉所有服务器, 我用这个服务器分配的这个 IP 地址 (广播)
 4. DHCP ACK 服务器确认分配 IP 地址

在 PC 上通过 `ipconfig /all` 也可以查到现在使用的 DHCP 服务器地址. 通常使用这个命令来确认到底在网络中使用的哪个 DHCP 服务器
```shell
# 一般在网关所在的设备上做 DHCP 服务器, 也可以在其他设备上去实现
ip dhcp pool NB_VLAN10
network 10.1.10.0 255.255.255.0 # 网络范围
default-router 10.1.10.254  # 网关
```

如果你有多个 VLAN, 上面这种方法就需要在每一个 VLAN 下面去配置. 一个优化的方案是在一个统一的服务器下面去配置. 然后在 vlan 下面做一个中继间, 把 DHCP Discover 广播转发到服务器上去

在 windows 上的服务器, 可以做 DHCP Server

DHCP 中继
DHCP 中继要成功运行
 1. DHCP 中继的包中带上了中继器的地址, 这个请求是从哪一个网段发来的. 应该从哪一个网段中取一个地址给它. 
 2. DHCP Server 必须有中继网关的路由
```shell
 interface vlan 20
 ip helper-address 10.1.100.100   # 把广播转换成单播发送给 服务器 10.1.100.100
```

DHCP 其实功能异常强大. 可以发很多东西, 包括 FTP 地址, FTP 密码 等等
043 Option 供应商特定信息. 应用场景. 比如大兴机场有几千个无线 AP, 每个 AP 都要自动分配 IP 地址, 且每个 AP 都要知道自己的控制器在哪里, 并自动到控制器注册? 北京上海的航站楼有 3000 多个, Disney 有 5,6 千个 AP
还能配置路由. 

DNS
在容器环境, 或者是公有云的环境, 在很多情况下是不允许你配置 IP 地址的
DNS 是一个分布式数据库, 不是一个 DNS 服务器知道所有的数据库
.com 是一级域名
example.com 是二级域名
www.example.com 是三级域名

DNS 无论是根服务器, 还是二级域名解析服务器都有 DNS 缓存. 也就是说如果最终的服务器 ip 地址改了. 但是二级域名解析服务器的 DNS 缓存没有更新, 就会把流量指向老的 IP 地址

所以很多时候网不同, 也可能是 ISP 的 DNS 服务器被毒化了. 解决这个问题方法, 是把主机的 DNS 的服务器指向还没有被毒化的 DNS 服务器

排障思路, 如何判断一台设备是否能正常上网, 不用 ping, 而是用 DNS 解析. 只要 DNS 解析成功, 就一定能上网, 如果 DNS 解析不成功, 就算是网络没问题, 也上不了网  

如果是在互联网上, 一般是找阿里或者腾讯申请一个域名. 
同一个 IP 地址, 可以有多个 A 记录. 比如 www, ftp
DNS 的记录类型
 
MX 邮件记录 collinstck@qytang.com 怎么找到 qytang.com 的邮件服务器在哪里
CNAME 别名 
NS 授权 DNS 服务器, 告诉那个服务器是真正意义上的授权
PTR 从 IP到域名
TXT 免费申请 SSL 证书时使用. 你是管理员才有权限做一个 TXT 记录. 然后腾讯会验证一下证书中的字符串. 验证成功后就会发证书给你

PC上做 DNS 解析
```shell
# 查看自己的 dns 服务器, 可以自动获取, 也可以手工配置
ipconfig /all 

# 连接上 dns 服务器, 做解析测试
nslookup
www.qytang.com
ftp.qytang.com

# 手工切到其他 dns 服务器上去
server 8.8.8.8
server 1.1.1.1
wwww.cisco.com

set type=a
set type=mx
qytang.com # 查询 qytang.com 的邮件服务在哪里
exit # 退出

```

一般情况下, 通过 DHCH 来发放DNS服务器地址

```shell
ip dhcp pool EI_VLAN10
 dns-server 10.1.100.100 
 
do sh run | section ip dhcp # 查看 dhcp 配置
```

在 PC 上重新获取 DHCP, 测试 DHCP 是否正常工作
```shell
ipconfig /release # 释放现在的 DHCP
ipconfig /renew # 请求一个新的 DHCP 地址

# 清空 DNS 缓存
ipconfig /flushdns
```

在 PC 机上也可以手工做 dns 本地映射(主机映射). 也就是说在没有 dns 服务器的情况下, 可以用本地手工映射的方式来解决. 

下面是在 PC 上手工指定 DNS 服务器
```shell
# 用管理员方式打开
c:\\Windows\System32\drivers\etc\hosts

10.1.100.100 ei.qytang.com 
```

下面是在思科设备上指定 DNS 服务器
```shell
# 激活 dns
ip domain lookup 
ip name-server 10.1.100.100
ip domain name qytang.com # 指定了域名后, 可以直接 ping www. 不需要 ping www.qytang.com

show hosts all # 查看现在解析到的
clear host all *  # 清楚 dns 解析
```