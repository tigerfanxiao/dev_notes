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
Media
* Fiber Optic circuit
光纤一般是两股, 一个收, 一个发
Each fiber-optic circuit is actually two fiber cables
* Coaxial 同轴电缆
* twisted-pair
    * Unshielded twisted-pair (UTP) 在北美使用
    * Shielded cables (STP) 在欧洲使用(在EMI和RFI高干扰场景下使用)

双绞线要考虑, 线缆之间的相互干扰
crosstalk: 线缆长距离绑在一起的干扰

双绞线配 RJ45水晶头

| UTP类型  |  速率 |
| ------------ | ------------ |
| 5类  | 100 Mbps  |
| 5类e  | 1000 Mbps 100 MHz |
| 6类  | 1000 Mbps at 250 MHz |
| 6类a  | 1000 Mbps at 500 MHz  |
| 7类e  | 10 Gbps at 600 MHz |

双绞线线序
* T568A
* T568B
![pin-scheme](twisted-pair-pin-oder.PNG)
双绞线的连通方式
* like-device: crossover
* unlike-device: straight-through

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


**延迟**
Delay是指网络两个节点之间数据传输的延迟, 用Latency来度量(数据从一个节点到另一个节点所需要的时间)

What are the most used wireless technology?
WiFi, Bluetooth, NFC (Near Field Communication), GPS

> **bit 和 Byte的区别**
bit就是单个0, 1 位. 只是表示信号
Byte由8个bit组成, 用来表示数字和字母
在网络传输中一般用bit来标记数据量. 


# windows命令
```shell
ipconfig
ipconfig /all # 查看详细信息包括, dns, dhcp server, lease
nslookup  # dns


```
ping 和 traceroute
```shell
ping url/ip
tracert url/ip
```
ping是基于icmp 协议
**禁ping的情况**
* 设备本身是block icmp echo request. 
* 设备前面的防火墙禁ping

**traceroute机制**
traceroute也是基于icmp协议. 首先发ttl=1的请求到网关, 网关把TTL改成0, 返回. 则第一条成功验证. 然后发TTL=2的请求, 得到验证后发送TTL等于3的请求

point-of-presence (POP) of ISP. The POP devices connect users to an ISP network.
