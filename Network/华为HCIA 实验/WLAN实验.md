
# 场景描述
1. 实现AC和两个AP的二层互联. 业务流量不经过AC, 走核心交换机
2. 核心交换机下连两台AP, 且有有线连接
3. 允许无线终端设备连接两台AP组成的BSS


# 拓扑

![[Pasted image 20230703010610.png]]

# 配置

# 总体配置步骤
1. 配置交换机, AC和AP的互联
2. 在AC上配置wifi


### 核心交换机配置
1. 在交换机上创建3个vlan
	1. vlan 10 用户给物理连线的用户使用
	2. vlan 20 用于AC和AP之间的管理流量. CAPWAP流量不打标签
	3. vlan 30 用户给无线终端的流量, 这些流量会被打上标签30
2. 开启dhcp
	1. 给AP分配ip地址
	2. 给无线终端分配ip地址

```shell
sysname SW_CORE

dhcp enable  # 开启DHCP给AP分配IP地址
vlan batch 10 20 30

ip pool sta # 交换机给给无线终端分配地址
gateway-list 192.168.30.1
network 192.168.30.0 mask 24

ip pool vlan20  # 创建global地址池
gateway-list 192.168.20.1 # 网关为AC的地址
network 192.168.20.0 mask 24

# 作为vlan10的网关
interface vlanif10
ip add 192.168.10.1 255.255.255.0

# 给AP分配IP地址, 否则无法被AC发现, 地址和AC在同一个广播域内
interface vlanif20  
ip add 192.168.20.1 255.255.255.0
dhcp select global  
# 根据当前接口ip地址所在的网段, 找到已经设置的地址池, 进行ip地址分配

interface vlan 30
ip add 192.168.30.1 255.255.255.0
dhcp select global # 给无线用户分配IP地址

# 对端接主机
interface g0/0/1
port link-type access
port default vlan 10

# 对端接AP1, 
# 带外管理, 必须要配置pvid出去vlan20的标签, 因为CAPWAP是不用标签的. 
int g0/0/2
# 必须配置成trunk, 因为管理流量vlan20和业务流量vlan30都要走这个接口
port link-type trunk
# 配置默认vlan是20, 这是AP的管理流量. 如果收到流量打上20的tag, 出去的话, 剥离20
port trunk pvid vlan 20 
port trunk allow-pass vlan all # 放通所有vlan, 2 到 4094

# 对端接AP2
interface GigabitEthernet0/0/3
 port link-type trunk
 port trunk pvid vlan 20  # 这里是capwap通信
 port trunk allow-pass vlan all  # 这里是无线流量, 设计是打上valn30的流量

# 用access口和AC对接
int g0/0/4
port link-type access
port default vlan 20

```


### AC的配置

```shell

# 因为 AC和AP通过vlan 20互联
vlan batch 20
# 在vlan 20中和AP交互
interface GigabitEthernet0/0/1
 port link-type access
 port default vlan 20

# 这个接口是用来和管理的
interface Vlanif20                        
 ip address 192.168.20.200 255.255.255.0
# 需要配置一条默认路由指向SW_CORE
ip route-static 0.0.0.0 0 192.168.20.1

```

### 在AC上配置wifi
1. 测试全网pc, 核心交换机和AC互联
2. 下面是指AC的wifi, 在实际组网中, 多半是通过web页面设置 

```shell
# 进入wlan模式
capwap source interface vlanif 20  # 设置AC和AP建立隧道的接口, 只有vlan20才会相应AP request的报文
 
wlan  
ap auth-mode no-auth   # 设置ap接入的时候不做认证. 在现网中, 可以是用mac地址准入认证  
regulatory-domain-profile name 123   # 创建域配置模板123
  country-code CN # 设置国家码
ap-group name default   # 创建默认ap群组名为default
  regulatory-domain-profile 123  # 给a p群组配置域配置模板 123

# 设置wifi实例
ssid-profile name n1  # 创建ssid配置模板 n1
ssid shuan   # 设置 ssid为shuan

#设置密码方式
security-profile name n2  # 创建密码登录模板 n2
security wpa psk pass-phrase 00000000 aes  # 设置密码和加密方式

# 设置vap 配置模板
vap-profile name n3  # 创建模板 n3
forward-mode direct-forward  # 本地转发, 默认可以不配
service-vlan vlan-id 30   # 无线用户流量进入的vlan
security-profile n2   # 绑定安全密码信息
ssid-profile n1 # 绑定ssid名称

#针对ap组下发
ap-group name default  # 进入到默认的组，默认ap上线都在里面
vap-profile n3  wlan 1 radio all   # 关联vap模板到所有射频卡, 这里wlan id可以是16个, 表示一种策略

```

### 效果检查

```shell
# 在AP上可以查询设备卡
dis int br
其中2.4G是Radio 0/0/0
5G是Radio 0/0/1



# 在 AC上查询
display station all
display ap all 
```



# 排错

1. 如果STA没有获得IP地址, 则在手机上看到的图标是一个感叹号
2. 