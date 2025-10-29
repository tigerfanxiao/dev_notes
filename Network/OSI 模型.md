OSI 是 ISO 国际标准化组织制定的
OSI 模型主要是分为
1. 物理层,  将数据转换为 bit 流, 提供机械和电气的规约. 放到传输介质上传输 PDU 单位是 bit 
2. 数据链路层, 将数据包封装成帧, 然后进行物理寻址, 差错检验PDU 单位是帧. 协议PPPoE, Ethernet, PPP
3. 网络侧: IP 寻址和路由探测 PDU 单位是包, 协议有: ICMP, IP, IGMP
4. 传输层: 建立, 维护和取消一次端到端的数据传输过程. 控制传输节奏的快慢, 调整数据的排序等
5. 会话层: 在通信双方之间建立, 管理, 终止会话. 类似操作系统给不同的应用分配一个进程号, 要来区分不同的进程
6. 表示层: 对数据进行格式转换, 已确保一个系统生成的应用层数据能够被另外一个系统的应用层锁识别和理解. 也包括一些加密解密, 编码, 文件后缀代表了解码的方式
7. 应用层: 对应用程序提供接口 ftp, smtp, pop3, Telnet, FTP, DNS,DHCP, HTTP, SMTP, DNS


# 物理层 
物理层规定了传输的[[接入网介质]] 比如是电缆还是光缆传输, 如果是电缆, 端口是什么规范. 如果是光缆, 光缆的端口又是什么规范. 
同样是光缆, 是不是又有不同的种类, 他们的传输距离不同. 
在通信领域, 负责这部分工作的一般称为传输和接入, 简称为传接.

当数据在网线中传输的时候, 因为是电信号, 容易收到强电的干扰, 或者是线缆之间的干扰. 但是物理层本身并不能解决因为干扰而出错的问题. 基于本层的问题由上层来解决的原则, 我们在数据链路层来实现对 bit 流的的校验. 

# 数据链路层
在数据链路层, 当我们收到物理层的 bit 流时, 我们会把 bit 流切割成一小块一小块. 
每一块. 每一块我们称为帧 Frame. 我们对每一个帧内部的数据做一次校验和, 来确定帧中封装的数据没有在物理层传输的过程中发生了篡改或者损坏. 
同时, 我们可以分辨这个帧从那个节点发过来, 又要到哪一个节点去. 这里就有了地址的概念. 
在链路层, 我们的地址称为 mac 地址. 值得注意的是, 这里我们说的节点并不一定是一台设备. 事实上, 一台设备的每一个接口都有一个 mac 地址. 如果这个设备只有一个接口,比如一台电脑主机, 那么它只有一个 mac 地址. 但是如果是一台交换机, 它有 24 个接口, 那就有 24 个 mac 地址. 在华为的设备上可以 `display interface`来查看

在数据链路层要如何实现通信. 是基于ARP 协议. 

最小的帧长度的是 64 字节, 范围是 64-1518. Data 是可变的, 长度在 46-1500之内

设备收到帧之后会如何处理
1. 如果收到是广播 mac 和组播 mac, 就是接受做下一布处理
2. 如果目标地址是单播的, 但是不是自己的就会丢弃
3. 如果目标地址是自己的, 就会完整性的校验, 如果数据帧完整就剥掉帧头部, 交给上层处理

网络设备如何确定以太网上层协议
通过数据帧中的 type 字段来确定上层协议 
0x0800 就是 ip 协议
0x0806 就是ARP 协议
0x86dd 就是 IPv6 协议

# 网络层
在定义 OSI 模型的时候, 用的是 CLNP, 而不是 IP. 


# IP包头部
整体的包结构
```
| L2 tailer | Data | L4 header | IP header | L2 header|
```

IP 包的头部是可变长的, 最小是 20 个字节, 最长是60 个自己
```

| version of IP | IHL | DSCP | ECN | Total Length      | 
| identification             | Flags | Fragment Offset |
| TTL | Protocol | Header Checksum field               |
|                  Source IP                           |
|                Destination IP                        |                      
|                   Optional                           |
```
字段说明
* version: ipv4 = 0100, ipv6 = 0110, 长度是 4 个 bit
* IHL Internet Header Length 长度是 4 个 bit. IPv4的包头是可变长的, 所以需要再 IHL 位置说明整个IP 头部的长度. 这个字段的最小值为 5, 表示 20 个字节, 最大为 15 表示 60 个字节. 4-byte increment
* DSCP = Differentiated Services Code Point 长度是 6 个 bit, Used for QoS (Quality of Service)
	used for prioritize delay-sensitive data (stream voice, video) 如果你加载网页慢一些, 不会有太大的影响, 但是如果你打电话或者视频通话卡顿就会有很严重的影响, 所以这里要调节 QoS
* ECN = Explicit Congestion Notification 长度 2 个 bit. 在不掉包的情况下, 提供端到端的网络拥塞信号. 这个字段是可选的, 需要底层两端设备都支持. 
* Total Length 长度是 16 个 bit, 表示整个IP 包的长度 = L3 header + L4 segment. 最小值是 20, 代表 20 个字节, 是最小的 IP 包长度, 其实包里面没有payload. 最大长度是65535
* identification  长度是 16 个bit, 差错检验. 如果一个 ip 包被分片了, 这里标注了, 这个分片是属于哪一个 ip 包的. 属于同一个 ip 包的分片, 这里的字段的值是相同的. 超过 MTU 的包都会被分片.  一般情况下 MTU 的1500 bytes
* Flags 长度是 3 个bit. 第一个 bit 位永远是 0, 第二个 bit 位称为Don't Fragment (DF)位, 如果这个位设置为 1, 表示不要对包进行分片. 第三个 bit 位称为More Fragment, 如果这个位设置为 1, 表示还有数据这个包的分片. 如果设置为 0, 表示这是这个包的最后一个分片了. 
* Fragment Offset 标识当前的分片在原来的包中的位置
*  TTL = Time to live . 有个 8 个 bit. Used to prevent infinite loops. 推荐的默认值是 64. 如果L4是 ICMP, 这里会设置成 255
*  Protocal 8bit 标记 L4 协议, 比如 TCP = 6, UDP = 17, ICMP = 1, OSPF = 89
*  Header Checksum 设备会计算 Checksum of header and compare with this field. 注意: 这里只是校验 IP 头部的信息. IP的 payload 的检验是通过 TCP 和 UDP 内的检查机制来实现.
* Optional Field 长度为 0-320bit

DSCP 和 ECN 加在一起的 8 个 bit也称为 TOS 位

# 传输层
决定消息是可靠传输还是不可靠传输
5元组来唯一的表示端到端的通信
SIP + DIP + (TCP/UDP)  + SPort + DPort
这里的源端口, 可以起到复用 ip 地址的目的. 比如同一个浏览器, 开两个 tab 访问google, 只要分配不同的源端口, 那么远端服务器能将两次访问给区分开. 


# TCP/IP
### TCP/IP 标准模型 4 层
1. 网络接入层
2. 英特网层
3. 主机到主机层
4. 应用层

OSI : 应用层 + 表示层 + 会话层 = TCP/IP 应用层
OSI:  数据链路层 + 物理层 = TCP/IP 网络接入层

### TCP/IP 对等模型 5 层
把标准模型的网络成拆开为数据链路层和物理层

应用层: Telnet 23 , FTP, SNMP, HTTP, SMTP, DNS, DHCP, BGP, RIP
传输层: TCP, UDP, OSPF
网络层: ICMP, IGMP, IPv4, IPv6, ISIS, VRRP, ARP
数据链路层: PPPoE, Ethernet, PPP, FR, ATM, HDLC, FDDI, 802.11 无线, x.25

>SMTP只用于发送邮件. 
>POP3是用来收邮件
>802.1 x 一种安全网络的准入协议, 不属于链路层


FTP 有两个端口 20 作为传输, 21 作为管理

为什么说 OSPF 是传输层的?
为什么 BGP 和 RIP 是应用层的?


# 标准化组织
### IETF
RFC 是他们搞的, 主要 TCP/IP协议族

### IEEE


### ISO