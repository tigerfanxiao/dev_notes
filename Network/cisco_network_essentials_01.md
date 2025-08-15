# 概念
### 单位
1 milion: 1M
1 bilion: 1G
1 trilion: 1T

### 网络定义
LAN: Loca Area Network 小型的局域网络, 使用Ethernet以太网技术
Internet: network of network 所有小型网络的集合

### Ethernet以太网
**网卡**
设备通过网卡 Network Interface Card (NIC)来连入以太网. 
所有的NIC都有唯一的MAC地址

### 网络的类型
无线网络
**蜂窝网络** The most common type of cellular telephone network is called a GSM network, an abbreviation of the title “Global System for Mobile Communications”


**无线网络的演进** GMS, 3G, 4G, 4G-LTE, 5G
注意:4G和5G之间由4G-LTE
### 网络的拓扑类型
peer-to-peer network 点对点网络: 由两台同时能收发的设备组成的网络, 两台设备互为Server和Client
peer-to-peer application: 这个应用同时收发数据, 比如聊天工具

> Server: 一般指发送数据的一方
> Client: 一般指请求或者接受数据的一方

### 网络设计
物理 vs 逻辑拓扑


### 网络的组成
**网络中的组件**
End User Device + Media(媒介) +Intermediary Device (网络设备)
Intermediary Devices = Networking Devices

**LAN的组成**
* Hosts/End User Devices (网络打印机被认为是host, 由自己的ip地址)
* networking devices
* Peripherals (连接在host上, 依赖host传输数据, 但是没有独立的ip地址, 比如本地链接打印机, 摄像头)
* Network Media (包括线缆, 空气)

### 数据的分类
Date Type: We can define 3 data types: 
- Volunteered Data 由人类输入的
- Observed Data 由传感器等度量得到
- Infered Data 计算推断出的

### 网络的度量

Bandwidth vs Throughput
The bandwidth count the number of bits can be sent cross the media in a second, which is basically theoretical metric. 
bandwidth的单位是bps
Throughput measures the transfer of bits across the media over a given period of time. 
Many factors influence throughput including the amount of data being sent and received over the connection, 
the types of data being transmitted, and the latency created by the number of network devices encountered between source and destination. 
Latency is the amount of time, including delays, for data to travel from one given point to another.


**延迟Latency**
延迟是指网络两个节点之间数据传输的延迟, 用Latency来度量(数据从一个节点到另一个节点所需要的时间)

**抖动**
抖动是指延迟的变化. 比如一个时间点上链路的延迟是 10ms, 另一个时间点上同一个链路的延迟还 15ms. 那么我们称抖动为 5ms


What are the most used wireless technology?
WiFi, Bluetooth, NFC (Near Field Communication), GPS

> **bit 和 Byte的区别**
bit就是单个0, 1 位. 只是表示信号
Byte由8个bit组成, 用来表示数字和字母
在网络传输中一般用bit来标记数据量. 


# Trouble Shooting on Windows
```shell
ipconfig
ipconfig /all # 查看详细信息包括, dns, dhcp server, lease

# dns 解析
nslookup www.baidu.com

# 刷新dns
ipconfig /flushdns 

# ping 测试
ping www.baidu.com

# 微软的tracert 是用 icmp 协议的
tracert url/ip
tracert -d -w 100 www.baidu.com
# traceroute 是用 udp 协议的

```

ping是基于icmp 协议
**禁ping的情况**
* 设备本身是block icmp echo request. 
* 设备前面的防火墙禁ping

**tracert机制**
traceroute也是基于icmp协议. 首先发ttl=1的请求到网关, 网关把TTL改成0, 返回. 则第一条成功验证. 然后发TTL=2的请求, 得到验证后发送TTL等于3的请求

point-of-presence (POP) of ISP. The POP devices connect users to an ISP network.

