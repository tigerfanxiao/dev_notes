[参考视频](https://www.bilibili.com/video/BV1eb411j7Wp/?p=4&spm_id_from=333.880.my_history.page.click&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
Quality of Service

基调: 理论复杂, 配置简单! 取舍(可以管理的不公平, 一类业务的提高会影响另一类的业务的服务质量)

工作中用的比较多的点
1. 限速或者是队列

QoS 五个基本职能
1. 分类和标识 Marking
2. 拥塞管理 (队列)
3. 拥塞避免 RED
4. 限速 (监管 policing, 整形 shaping)
5. 链路效率

QoS 的分类站在TCP/IP角度来分类
1. 2 层 QoS
2. 3 层 IP 的 QoS
3. MPLS 2.5 层的 QoS

QoS 要解决的问题
1. 带宽 bandwidth  FTP 对带宽有一定要求
2. 时延 delay VoIP 语言流量对时延要求比较高
3. 抖动 jitter 不平均, 不平稳的时延
4. 丢包 drop 尾部丢弃


端到端的 QoS
最大带宽是路径上的最小带宽 -- 木桶理论

速率
clock rate 物理上的, 时钟频率
bindwidth 逻辑上, 影响选路


时延
1. 传输时延或串行化时延 Propagation or Serialization delay
2. 处理时延
3. 队列时延


队列
软件队列 PORQUE  Priority Queue 优先级队列
硬件队列 - 基本上都是 FIFO, 不需要操控


丢包
1. 尾部丢弃 队列满了丢弃
2. 主动丢弃 WRED 的丢弃
3. WFQ的丢弃

用丢弃概率来防止拥塞




增加带宽的方法
1. 升级带宽, 要花钱
2. 配置优先传输的报文
3. 压缩帧

IP 20 字节, TCP20 字节, 数据 2K
时延 = 带宽 / 速率

对丢包敏感的数据提供足够的带宽, 比如语言流量
在拥塞发生之前,提前随机丢弃一些不重要的报文来避免拥塞, 也称为 WRED Weighted Random Early Detection



吞吐率

Ineractive 交互式的, 不需要大吞吐量, delay 也不高, loss, 
batch 比如 FTP, 吞吐率要求高, 时延不重要
Voice 吞吐量不高
Video 吞吐量高



QoS 的服务模型
1. Best Effort 尽力而为, 没有 QoS 
2. 集成服务模型 RSVP, 可以用于 VOIP和 MPLS 流量工程, 一个致命缺点是, 基于软件, 本身没有 QoS 保证
3. 差分服务模型 Differentiated Service -- MQC 模块化的 QoS 配置
	通过设置报文头部的 QoS 的报文参数来控制

分类原则
1. 2 层头部 802.1Q中的 802.1P
2. 3 层的 IPv4 头部(3 位 IP 优先级, 6 为 DSCP 位)
3. 3 层的 IPv6 头部 (Traffic Class)
4. MPLS 的 EXP 位


802.1Q
CFI | Vlan | 802.1P |

差分服务的边界节点, 在节点外没有 QoS

PHB DSCP 一共 6 为
Per hop behavior 每一跳行为
4 类
1. Default, Best Effort
2. Class-Selector  DSCP 的高 3 为 IP 优先级, 后 3 位为 0. 
3. Expedited forwarding  EF位101110 快速转发, 立即做  
4. Assured Forwarding  AF保证转发, 会做, 但是要等. 会有管制流量, 超过的会被丢弃

因为 IP 优先级有 3 位, 所以一共定义了 8 中优先级
0-routine
1-priority 数据data 优先
2-immediate 数据
3-flash 如果把 3 映射为 DSCP 一般是 24, 乘 8 的关系
4-flash-overide 急速,用于 video
5-critical  voice 使用 101
6-internet  分配给路由协议来使用
7-network 分配给路由协议来使用

dd位是丢弃可能性, 值越大, 越容易被丢弃 包含 01, 10, 11, 一共是 12 种 
AF1 dd
AF2 dd
AF3 dd
AF4 dd


AF43 丢弃可能性大于 AF21, 因为 3>1 
拥塞避免 Congestion avoidance 针对不同的 AF 有不同的待遇

