
# 基本概念
防火墙和路由器的作用不同之处在乎. 路由器控制但是不同网段之间的路由. 防火墙增加去 zone 区域的概念. 防火墙可以控制不同的区域之间的流量. 

### 不同形式的防火墙
* windows防火墙
说白了就是ACL, 允许了某些应用. 入站和出站规则


### 企业对安全的诉求
1. 外部网络的安全隔离
2. 内部网络的安全隔离
3. 入侵防御
4. 内网的行为管理, 内容安全的过滤
5. 防病毒
### 安全区域 Security Zone
默认情况下区域分为 Trust, Loca, Untrust 和 DMZ, 当然还可以自定义区域. 
**注意** 防火上的接口本身属于 Local 区域, 把某个接口划分到某个区域指的是, 把这个接口下面连接的所有网段中包含的设备划分到这个区域, 并不包含接口本身.
* 防火墙默认每个区域之间是不通的， 所以防火墙的策略是区域间 Interzone 的策略
* 防火墙的策略是有方向的
	DMZ 区域不能访问 Trust 区域. 但是 Trust 区域可以访问 DMZ 区域. 只有防火墙能做到这种单向的区域控制

区域默认优先级
注意: 在现在的防火墙中这些区域的优先级意义不大

| 区域       | 安全优先级 | 注释           |
| ---------- | ------ | -------------- |
| Local 本地 | 100    | 防火墙上的接口 |
| Trust      | 85     | 公司内网       |
| DMZ        | 50     | 公司服务器     |
| Untrust    | 5      | 外网           |

### 防火墙的作用
* 隔离不同安全级别的网络
* 实现不同安全级别的网络之间的访问控制(安全策略)
* 用户身份认证
* 实现数据加密及虚拟专用网业务
* 执行网络地址转换
* 其他安全功能 IPS

### 防火墙产品的其他组件
防火墙除了硬件之外, 还有其他附加功能, 是通过 License 来卖的

IPS 入侵防御检测功能
现代的防火墙, 这部分已经不是硬件了, 而是防火墙上的license

### 会话表
防火墙通过会话表来控制流量. 只有在会话表没有命中的情况下, 才能查询路由表

### 安全策略
基于 5 元组
- mac
- ip
- 协议号
- 端口号
- 用户, 时间
防火墙的动作
- 允许
- 拒绝
- 反馈/不反馈
- 流量清洗  面对重放攻击
- 防病毒
一般的配置方法, 先做全允许. 这样可以做测试. 否则按照默认的配置是全部拒绝的
后期再根据具体的需求, 再写策略
permit any any

条件
1. VLAN ID
2. 源安全区域
3. 目的安全区域
4. 源地址/地区
5. 目的地址/地区
6. 用户
7. 服务
8. 应用
9. URL 分类
10. 时间段
微信你发的图片都能知道

安全配置文件(看你购买的服务)
- 反病毒
- 入侵防御
- URL 过滤
- 文件过滤
- 内容过滤
- 应用行文过滤
- 邮件过滤
- APT 防御
- DNS 过滤
### 防火墙的性能
防火墙可以对交换的报文进行安全检测, 包括数据链路, 网络, 甚至报文中的内容. 
因为要检查报文, 所以一般情况下要不影响网络速率, 防火墙的性能是要比交换机和路由器更高的

防火墙有个独特的硬件叫SPU Service Processing Unit
路由器里有个模块叫 LPU Line Processing Unit 包含 ACL规则, QoS策略, 流量管理

### 华为防火墙的初始配置
1. 华为防火墙有默认密码
```shell
admin
Admin@123
# 建议修改密码 huawei@123
```

2. 防火墙的0/0/0口是管理口
	1. 无论是虚拟机还是真机在出厂的时候都是有网管地址 192.168.0.1 
	2. 如果要通过页面配置防火墙, 需要在笔记本上配置自己的网卡地址 192.168.0.11, 并将网线连接防火墙上的 0/0/0 口和笔记本的网口
	3. 目前管理页面是 http 的协议
	4. 管理口默认情况下是放在 vpn-instance default 下面的, 和业务路由表隔离. 所以不用担心它和业务网段冲突
	5. 防火墙的管理口一般不走业务流量. 业务流量一般从 1/0/1 口开始

**注意** 管理口配置完ip地址后, 默认是ping服务没有打开, 是ping不通的

```shell
sys
int g0/0/0
ip add 192.168.1.50 24  # 修改管理页面的 ip 地址
service-manage ping permit # 放通 ping, 只有把接口分配zone之后才能ping通
service-manage http pemrit # 做实验的时候都开, 真机可以不开
service-manage https permit # web 管理防火墙 http://192.168.1.50
```

3. 划分区域
# Ping 测试
无论是从防火墙出去 ping 其他端口, 还是通过其他设备来 ping 防火墙, 都需要做策略才能实现

### 快速配置防火墙的某个接口可以被 ping
1. 发起 ping 的设备, 必须和这个接口在一个区域内

```shell
# 首先要确认这个接口会被那一

int g1/0/1
ip add 192.168.1.2 24
service-manage ping pemrit 
```


### 从防火墙 ping 外网
需要配置策略, 把
```shell
source-zone local
destination-zone untrust
```
### web登录
电脑上虚拟机配置web的登录需要先创建虚拟网卡

打开设备管理器, 添加过时硬件

![[Pasted image 20231108080905.png]]

![[Pasted image 20231108081921.png]]

![[Pasted image 20231108081951.png]]

![[Pasted image 20231108082022.png]]

![[Pasted image 20231108082102.png]]

![[Pasted image 20231108082130.png]]


通过云桥接虚拟网卡

![[Pasted image 20231108075248.png]]
配置云

![[Pasted image 20231108075153.png]]



telnet, 只有密码, 没有账户
默认在防火墙禁用
```shell
sys
telnet server enable
user-interface vty 0 4
 protocol inbound all # 或者telnet
 authentication-mode password
 set authentication password cipher huawei@123
 user privilege level 3 # 至少3级
# 进入网关口
int g0/0/0
service-manage telnet permit 

```

ssh
不创建新的用户
```shell
sys
user-interface vty 0 4
 protocol inbound all # 默认允许ssh和telnet
 authentication-mode aaa
 user privilege level 3

# 不用创建新的用户
ssh authentication-type default password
# 创建本地密钥对
rsa local-key-pair create
# 默认长度 2048

# 开启stelnet 服务
stelnet server enable
# 把管理的默认账号 admin 设置为ssh的登录用户
ssh user admin
ssh user admin authentication-type password
ssh user admin service-type stelnet

# 配置aaa
aaa
manager-user admin
service-type web ssh terminal

int g0/0/0
service-manage ssh permit
```
但是secure-crt还是会认证失败
如果用路由器去连接防火墙, 第一次需要敲. 如果用客户端是不需要这条命令的
```shell
ssh client first-time enable 
```


开模拟器时, 如果只是用傻瓜交换机, 可以把生成树协议关了, 提升性能
```shell
stp disable
```


软件版本不一样的防火墙
```shell
display version

```

huawei 企业网 e.huawei.com -> 技术支持 ->产品和解决方案支持 -> 安全





### 为什么允许外部访问的服务器要放在非DMZ中
公司内网的设备要访问外网一般是使用动态NAT. 这种情况下, 内网的地址外网是不可知的, 所以外网很难直接攻击某一台内网主机. 但是服务器的ip地址一般是用静态NAT的, 所以如果一旦被攻破, 变成肉鸡, 就很容易以服务器为根基, 攻击内容的主机所在的区域. 

在设计防火墙策略时, 一般不允许从DMZ区域直接访问内网主机所在区域. 这种策略只有防火墙能实现, 路由器和交换机无法实现. 


# 防火墙组网方式
1. 网络边界
2. 旁挂式组网. 可以提供安全增值服务(也称为安全联动), 让购买流量的用户流量经过防火墙 

### 双机热备
区分设备出现故障
接口出现故障
TCP在开启状态检测之后, 只有首包建立会话表项 Session

用传统的VRRP, 就无法实现, 主备防火墙的状态一致性. 因为每一个vrrp都是独立的切换的. 如果是设备损坏, 里面和外面会一起切换master. 但是如果只有里面的链路断了, 里面切换了. 外面没有切换. 就会造成回去的路由, 找不到nat的session, 因为session是在坏了的master上创建的. 且TCP在开启状态检测之后, 就只有首包会建立会话表项. 从而造成丢包.

各大厂商都有自己的双机热备的私有协议和方案. 

华为的私有协议叫VGMP VRRP group management protocol. 可以将多个VRRP组管理起来. 一个组发生了切换, 其他的组也切换
VGMP的Active设备, 是所有管理的VRRP组的Master设备. VGMP的standby设备, 所有管理的VRRP组的Slave设备

Active和standby设备的默认优先级是45000, 如果发现VRRP备份组中有master设备切换了, Active设备就会把自己的优先级减1. 并通告给Standby设备. Standby设备会把所有的备份组的master都切换掉


HRP 协议, 也是华为的私有的数据备份协议:
包括设备信息备份, 命令行备份, 各种状态信息的备份. 通过这个协议, NAT的session就在主备设备上同步, NAT的转换表, IPSec的加解密信息

HRP协议可以看做VGMP协议的子协议

HRP 配置方法

```shell

hrp enable # 在主备设备上都要开启. 在主备设备没有联通的情况下, 都是 standby
hrp interface g0/0/7 # 在主备设备上互相指对方是心跳口. 主要物理连线正常

int g0/0/7
ip add 192.168.2.1 30

firewall zone trust
add interface g0/0/7  # 两个

dis hrp state  # 查看 hrp 状态, 主设备会显示 master

```


### NAT 配置方法
1. 配置 address-group. 如果是两条出口链路, 需要独立创建两个zone

```shell

nat address-group group1
route enable 
# 这里指 192.168.1.2 到 192.168.1.5 三个 ip 地址
section 0 192.168.1.2 192.168.1.5 # 做源 NAT 转换

nat-policy
rule name policy-nat1
source-zone trust
destination-zone untrust
action source-nat address group1 # 这里引用 address-group
```


IP link 
探针检测

```shell
 
ip-link check enable
ip-link name to-ftth
 destination 8.8.8.8 interface wan0/0/0 mode icmp next-hop 192.168.4.1
# display ip-link # 查看ip-link是否配置成功

ip route-static 0.0.0.0 0.0.0.0 192.168.4.1 track ip-link to-ftth
ip route-static 0.0.0.0 0.0.0.0 192.168.0.1 preference 61 

nat address-group ftth
mode pat 
route enable 
section 0 192.168.4.242 192.168.4.242

nat address-group 5g
mode pat 
route enable 
section 1 194.168.0.242 194.168.0.242

nat-policy
rule name nat_ftth
 source-zone trust
 destination-zone untrust
 source-address 192.168.20.0 255.255.255.0
 source-address 192.168.30.0 255.255.255.0
 source-address 192.168.40.0 255.255.255.0
 action source-nat address-group ftth
 
rule name nat_5g
 source-zone trust
 destination-zone untrust
 source-address 192.168.20.0 255.255.255.0
 source-address 192.168.30.0 255.255.255.0
 source-address 192.168.40.0 255.255.255.0
 action source-nat address-group 5g

```