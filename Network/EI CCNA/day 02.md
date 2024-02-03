
vlan  802.1Q tag 有 12 个 bit
vlan 的作用是隔离广播域用的
vlan 目前定义的 vlan  0-4094 但是对于云计算场景的虚拟机不够用, 所以引入的 vxlan 技术

VLAN 技术的好处
1. 减少广播域, 降低 CPU 开销(收到广播的主机 CPU 来判断是否回复这个广播, 因为要操作系统级别, 才能调用 CPU, 特别是 UDP级别的广播), 提供主机性能. 所有听到广播的主机都会思考其中的问题
2. 安全隔离, 降低安全风险. 比如病毒在直连网络中传播最快
3. 更加灵活的设计. 只要登录交换机把接口划入那个 vlan 就行了

连接到同一个交换机到属于两个网段的设备互相是不能通信的, 需要通过路由器来实现

默认情况下, 所有接口都在 vlan1

2-1001 # 自己可以用. 1002-1005 思科给很老的技术预留的
1006-4096 扩展 vlan, 一些很老的设备不支持

组播的优势
1. 只有加入组播组的人, 才会被骚扰.
2. 收到组播的设备, 在数据链路层, 网卡就能够处理, 而不需要到 CPU 层面

包
作为一个网络工程师需要知道包长什么样, 交换机看一个包, 看什么位置. 路由器看什么位置. 
防火墙看包什么地方, 防火墙, 控制着包什么位置. 它怎么去决策到底放不放这个包过的

在 B站上教主有 wireshark 课
自己保存一些 wireshark 包, 自己抓包查看


思科路由器 Cisco 4321 
Console 口也是 RJ45的, 但是和网口有区别, 网口一般是有灯的(有数据传输就会闪). Console 口没有灯
Console 线也有线序, 但是全反线. 12345678, 87654321
现在有 USB 的 console 线. USB type a, 或者 USB type c
现在有些设备还是要装驱动的

AUX 口, 其实也是 console 口, 插 console 线效果一样. 远古的时候, 通过播电话号码来接入设备的, 接到 modem.  现在新设备一般没有 AUX 口了, 交换机一般不带, 老的路由器有可能有

网口分为普通的网关口和带外网管口
MGMT口, 带外网关口. 
普通口, 可以是电口, 也可以是光口. 有可能是插了点的, 就不能用光SFP

如果你需要远程访问一下设备, 你需要在网口上配置一个 IP 地址. 带内和带外的网管口都能配置 IP 地址
带外网管 out-band management: 生产流量和网管流量是独立的, 没有走在一起
带内网管 in-band management: 生产流量和网管流量是一起的

电信 DCN 网, 其实是带外网管网. 把所有的带外网管口接入一个独立的网络

思科高端的 4K 系列, 可以把服务器当做模块插在路由器上

思科 3560 老的交换机设备. 交换机一般前面板都是接入口. console 口可能在背面
现在好的交换机都是冗余电源

思科现在在卖的交换机就只有 catalist 9K 系列, 有 9200低端, 9400高端

常见交换机命令
```shell

show ip interface brief
show version
show privilege # 查看用户级别, 正常进来是 1 级, 最低是 0 级
enable # 进入特权模式 15 级

dir # 查看 flash 中的文件
show run # 查看配置

conf terminal # 进入全局配置模式


# 乾颐堂默认登录密码
enable password Cisc0123
# 如果是 windows
administrator/Cisc0123

# 创建 vlan

vlan 10
name office
show vlan brief # 查看 vlan 信息

# 删除 vlan的数据库. 注意即使删除所有 config, vlan 数据库还是会保留
delete flash:/vlan.dat

# 保存配置
copy running-config startup-config # 等同于 wr
wr

# 恢复保存的配置
copy startup-config running-config
```

保存配置
running-config 是在内存中的. 在 ios 中敲的命令立即生效. 这种情况下, 有可能敲一句命令后就立即断网. 因为敲一句生效一句
在现在的设备中有 commit 提交后一起生效. 
start-up config 保存在 NVRAM, 不是 flash 中, 掉电不会丢失. 比 flash 更安全. 一些重要的东西都保存在 NVRAM 里, 因为做了硬件加固的. 
NVRAM 非易失性存储器, 用于存储配置文件(安全加固)

Flash 哪些不那么敏感, 又比较大的文件保存在 flash 中, 类似硬盘. 比如 IOS 的二进制文件
ROM 只读存储器, 存储交换机的引导程序, 开机时引导交换机找到系统镜像文件. 类似于 BIOS

如果把 ios 文件删掉了, 就要用 TFTP 把文件拷贝系统文件, 做系统恢复

Juniper 还有一个 confirm 的命令. 就是 commit 之后生效. 如果在一段时间内没有回复 confirm, 设备则会自己回滚配置. 比如commit 之后立即断网了. 则会在一段时间内回滚配置


补齐命令
```shell
show a # 如果不能补齐, 表示有多个命令. 如果能补齐, 表示只有一个补齐方式
show a? # 补齐所有候选命令

# 华为会给你猜一个

ctrl + p # 上一个命令
ctrl + n # 下一个命令
ctrl + u # 删除整行
ctrl + p, ctrl + a # 光标到第一行, no掉一条命令

```


Trunk
主要用于接入交换机和核心交换机的互联. 实现跨交换机(跨核心), 相同 vlan 内终端的通信

```shell
# 创建 vlan 10
vlan 10
name office 
# 划分 vlan
interface f0/1
switchport mode access
switchport access vlan 10

# 把多个接口划分进入一个 vlan
interface f0/1-f0/3
switchport mode access
switchport access vlan 10

```