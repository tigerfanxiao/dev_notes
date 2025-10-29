# 行业发展
在国内， 从2016-2019年已经在三大运营商全网部署了
13台IPv4的DNS根服务器在中国一台没有， 但是IPv6中国就技术优势， 有根服务器

IPv4的分配机构是IANA， 在2011年已经分配完了
IPv4能使用至今主要依赖两种技术， 私网地址和NAT技术

问题： 很多地方已经开始使用二级NAT技术？ 私网 | 私网 | 公网


### IPv6优势
* 无限地址空间 IPv6 地址长度 128 位1=16字节, 是 IPv6 的 4 倍
* 层次化的地址结构
* 即插即用 SLAAC 无状态地址自动配置
* 简化的报文头部. IPv6 的报头比 IPv4 更有效率, 字段更少, 去掉了头部的校验和

### IPv6 过度技术
* 双栈技术 IPv4和IPv6并存 
* 隧道技术 将IPv6封装到IPv4隧道中， 反之亦然
* 转换技术 NAT64

### IPv6 路由协议
* OSPFv3 不支持IPv4 和OSPFv2不同
* IS-IS for IPv6 在原来的协议上扩展
* BGP4+
* PIM 组播

问题： IPv6 LLA 是什么？





无需 NAT
不再有广播 ARP, 用 ICMPv6替代

新扩展的头部取消了 Option 字段, 

### IPv4地址
1. IPv4的地址表示方法是点分十进制
2. Ipv4地址分为 网络位和主机位

IPv6 到 IPv4 的过渡方式

### IPv6 地址
长度128bit， 分为8段用冒号隔开， 每段16bit， 每一段4个16进制数字，一共32个16进制数，不区分大小写


### IPv6地址表示方法
1. 首选格式 原来的样子
2. 压缩格式
3. 内嵌Ipv4地址的方式

#### 压缩格式
`2001:DB8:0:1::45ff/64`
* 每一段前导0可以是省略， 如果该段全为0， 这保留一个0。 拖尾的0不能省略
* 如果多个连读段为0， 则用`::`表示，一个IPv6缩写只允许由一个`::`

#### 内嵌Ipv4地址方式
 `0:0:0:0:0:0:166:168.1.2/64`
* 前96bit为IPv6地址格式， 后32bit位IPv4地址
* IPv6部分可采用首选或压缩格式，IPv4部分采用点分十进制格式

### Ipv6地址结构
一个IPv6地址分为网络前缀和接口标示对应IPv4的网络位和主机位

|ipv6| ipv4| comments|
|----|----|----|
|网络前缀|网络位||
|接口标示|主机位||


### IPv6 报头
IPv6头部最大是 40 个字节

|  Version  |   Traffic Class  |   Flow Label       |
| Payload Length |  Next Header | Hop Limit |
|                     Source Address                         |
|                     Destination Address                  |

* Traffic Class 是用于 QoS , 类似 IPv4 报头的 ToS 字段
* Next Header 是用于指定上一层的协议, 类似 IPv4 报头的 Protocol 字段 + Option. 可以在头部后放置多个扩展包头, 这些扩展包头之后才是有效载荷
* Hop Limit 跳数限制, 类似 IPv4报头 TTL
* Payload Length 类似 IPv4 报头的Total Length

### 扩展包头
只有目的端设备才会完整的查看扩展包头
Next Header 结构
```shell
# 没有扩展包头, Next Header 直接指向上层协议
| IPv6 Header: Next Header = TCP | TCP Header | Data | 

# 有扩展包头, 
| IPv6 Header: Next Header = 路由 | 路由选择包头: Next Header = TCP | TCP Header | Data | 

```

扩展包头
1. 基本 IPv6包头
2. 逐跳选项报头
3. 目标选项报头
4. 路由选择报头
5. 分段报头
6. 身份验证报头AH, 封装安全有效负载 ESP 报头
7. 目的选项报头
8. 上层报头, TCP, UDP, ICMPv6 等

IPv6 报头的改进
1. 取消了 IP 的校验, 因为二层和 4 层都有各自的校验机制. 
2. 取消了中间节点的分片功能, 通过 PMTU 来发现路径 MTU(路径上最小的 MTU)
3. 定义最长的 IPv6 头部, 有利于硬件快速处理
4. IPv6 都 IPSec的完美支持, 如此上层协议可以省去很多安全选项, 如 OSPFv3 就取消了安全认证
5. 增加流标签, 提高 QoS 效率

### IPv6 的编址
