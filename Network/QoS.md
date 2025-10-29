[参考视频](https://www.bilibili.com/video/BV1eb411j7Wp/?p=4&spm_id_from=333.880.my_history.page.click&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
# 概念
### 什么是Quality of Service?
* 取舍(可以管理的不公平 Managed unfairness, 一类业务的提高会影响另一类的业务的服务质量)

### 如何实现 QoS
做标记的目的是对流量进行分类 Classification and Marking. 类似给乘客头等舱的票还是经济舱的票. 拿到头等舱票的流量, 会得到更好的服务.
1. 在信任边界设备上, 做标记. 标记可以在
	* 2 层, COS 位, 3bit, 在 802.1Q 中的 802.1P 位
	* 2.5层, MPLS Label Experiment 位, 3bit
	* 3 层, TOS 位, 8bit = DSCP + ECN
2.  在沿途的设备上, 对流量进行 remark. 本质上有个汇聚的过程. 也就是说越是接近核心网, 标记类别越少. 

	  
### NBAR Network Based Application Recognition
除了给流量做标记之外, 我们也可以根据包中其他深入的信息(4-7层), 来实现不同的策略. 但是NBAR 是 CPU intensive 
* IP 源目的地址  
* MAC 源目的地址
* 目的端口号 比如 HTTP 80 - Deep Packet Inspection
* Domain name 域名 比如 cisco.com 
NBAR有两种 mode: 
* Passive: Provide real-time statistics on application per protocol, per interface and give bidirectional statistics such as bit rate, packet and bytes count 不做 marking
* Active: Classifies application for traffic marking, so that QoS polices can be appliedx


### 现实中比较 QoS 方向
* Rate Limiter 限流 = Policers  + Shape 
* Queue 队列 用来拥塞管理 Congestion Management 

### Policer vs Shaper
* 一般来说 Police 当达到设定的带宽最大值时直接将包丢弃, 不做 buffer 或者 delay traffic. Shape 则会延迟 smooth traffic out by buffering it
* 现实中, Policer 可以做 2 个 threshold, 对低于 threshold 的流量放行, 不做标记. 对超过threshold 的流量依旧放行, 但是做标记. 当流量超过第二个 threshold 时再 drop. 这种模式也称为 tri-color implementation 
* Policer一般用于 ingress 流量, 因为流量在处理之前就 drop 掉了, 所以设备不用处理了, 就减少了设备的负载. 比如 ISP 的入口设备会有 Policier, 不让你上传太多的数据. 然后由 TCP的重传机制保证数据重传 Retransimission. Policer 不会造成 jitter 和 delay, 因为它就是丢包. Policer 一般用在个人用户和 ISP 连接的地方. 而 Shaper 一般是用在企业和 ISP 连接的地方, 以为 Shaper 更 smooth
*  Shaper 会造成 jitter和 delay. 不用做 TCP 重传. 

### CAR PIR CIR的概念
Committed Access Rate (CAR) is a metric used in Internet quality of service (QoS) agreements to classify and limit customer traffic and manage excess traffic according to the network policy. It is a rate-limiting feature that provides the functionality for policing traffic.

Peak information rate (PIR) is a burstable rate set on routers and/or switches that allows throughput overhead. Related to committed information rate (CIR) which is a committed rate speed guaranteed/capped.

### Trust Boundary
默认情况下, 设备不会相信别的设备发来的标记. 
比如企业网接入的层的交换机, 相信下游的 VoIP电话机发来的语音流量, 这是 VoIP 电话机就是 Trust Boundary, 但是如果有一台 PC 连接在电话机下方, 则 PC 发来的数据流量不可信任. 
又比如, 企业内部带有标记的流量不应该发往 ISP, ISP 也不会相信这些 marking. 于是这些出口设备也是信任边界. 
综上, 一般在网络的边界Edge of Network 上做标记.


### 常见的业务
要对不同的业务做标记, 以分类. 需要了解不同业务对延迟和丢包的容忍度
1. Data
	* 对丢包不敏感, 由TCP 重传机制来保障, 比如 FTP 给多少带宽就用多少带宽, 也称为 greedy. 也可能突然流量暴增, 称为 bursty
	* Delay Insensative. 但我们可以把 app 分为两类 Interactive 和 Non Interative. 对于要交互的应用, 比如 Telnet就对 delay 比较敏感
2. Video
	* Bursty, Greedy 动作片比静态图像更消耗带宽
	* 类似 voice, delay and drop sensative. 
	* 延迟< 150ms, jitter 小于 30ms, loss 小于 0.1~1%, bandwidth 384kbps-20Mbps
	* 一般用 UDP 来传输. 
3. Voice
	* Delay sensative, drop sensative
	* Require certain amount of bandwidth
	* 一般用 UDP 来传输, 所以不存在重传. 使用 RTP 协议
	* 延迟小于 150ms, jitter 小于 30ms, loss 小于 1%, bandwidth 30-128kbps
本质上, 我们会 prioritize  video 和 voice, 然后给 data 业务如FTP 降低优先级


### Tool set of QoS
* Classification and marking tools
* Policing and markdown tools
* Scheduling tools: Queuing, dropping
* Link-specific tools
* AutoQos Tools
* Call Admission control 电话是否允许被接入网络

QoS 五个基本职能
1. Classification & Marking 设置优先级
2. Congestion Management (队列)
3. 拥塞避免 RED
4. 限速 (监管 policing, 整形 shaping)
5. Link Specific Mechanism


QoS 要解决的问题
1. 带宽 bandwidth  FTP 对带宽有一定要求
2. 时延 delay VoIP 延迟低于 300ms
3. 抖动 jitter 不平均, 不平稳的时延
4. 丢包 loss 主动丢包/被动丢包


端到端的 QoS
最大带宽是路径上的最小带宽 -- 木桶理论

速率
clock rate 物理上的, 时钟频率
bindwidth 逻辑上, 影响选路


时延
1. 传输时延或串行化时延 Propagation or Serialization delay
2. 处理时延
3. 队列时延

# Queuing Mechanism 队列机制
Queuing 也称为 buffering. 用来解决拥塞. 因为如果没有拥塞的情况, 就不需要排队了. 当队列占满的时候, packet 会被重排. 高优的包会被先发出去
一般关注流量出去的方向. 

队列机制有多种, 不少队列技术已经不适合现在的网络环境. 
* Round Robin Queuing Mechanisms 所有人都被相同的处理
* Strict Queuing Mechanisms 严格优先队列
	Strict Queuing Mechanisms 的问题是, 如果有很多高优的流量, 则会占据整个队列, 使得低优的流量根本无法通过. 
* FIFO First In First Out 用于 single queue
* PQ Priority Queue 一般被分为 4 个优先级 High, Medium, Normal, Lower. 只有当High, Medium 的队列处理完了, 才会处理 Normal 的队列. 同理 Lower 队列, 只有在其他高优队列空了之后才会处理. 这就造成一个问题, 如果高优包很多, 高优队列一直没有空. Lower 队列就永远都不会被处理
*  CQ Custom Queue 有 16 个队列, 每个队列占据的带宽可以不同. 通过 round robin 轮询, 有 traffic guarantees. 问题在于对高优流量没有保障. 因为要等 16 个队列都空出来, 才会接受新的包做下一轮的轮询. 引入delay 和 jitter

所以, FIFO, PQ, CQ都不适合现代网络
* WFQ Weight Fair Queuing 没有bandwidth quarantee. 通过源目的地址, 协议, 端口号, 流量被分为不同的 flow. 根据 IP Precedence, RSVP 给 flow 分配权重. 一般给小的 packet 分配更多的权重, 因为小的 packet 一般是用于 interactive session. 比如 voice packet 一般是 20 字节, 而 FTP packet 一般是 1500 字节. 
* CBWFQ  Classed Based Weight Fair Queuing 思科私有, 提供 bandwidth quarantee. 问题是没有latency guarantee, 因为没有优先队列 - 只适用于传输数据的网络
* LLQ  Low Latency Queuing 事实上是对 CBWFQ 的扩展. 里面在 CBWFQ 的基础上, 另外增加了一个 PQ. 这个 PQ 给 real-time traffic 用, 有最小和最大带宽保障, 以至于starve 里面的 CBWFQ队列. 

# WRED Weighted Random Early Detection 
是一种拥塞预防机制 Avoiding congestion. 或者 Scheduling tool

设备的资源是有限的. 如果遇到 burst of traffic, 或者 buffers are overrun. 当队列占满时, 所有新到的包都会被丢弃, 这种机制成为 Tail Drop. 
Tail Drop会有问题. 假设有多个 flow 在传输数据(传输其实节点可能不同), 在队列占满时, 这时启动 Tail Drop 机制, 所有Flow 的包都被丢弃. 因为TCP 重传机制, 每个 flow 的 TCP 会增大传输的时间窗口, 增加传输包的个数. 因为这些 flow 都是在同一个时间点上进行调整, 所以很可能调整后的结果是, 各个 flow 又再同一个时间点开始发送数据. 这样造成这个时间点的流量特别多, 其余时间点的流量减少. 这个现象被称为 Global Synchronization. 
WRED 会随机挑出几个 flow, 在队列占满之前, 随机进行丢包. 这样, 没有被丢包的 flow, 会加快传输的速度, 被丢包的 flow, 会减慢传输的速度. 如此将不同 flow 传输包的时间区分开, 从而来避免Global Synchronization 

> TCP 的传输机制是, 当方式丢包时, 会增大时间窗口, 传输更多的包. 但没有丢包时, 会缩小时间窗口, 加快传输的速度.
> 

WRED 为每一种分类(基于 IP Precedence 或 DSCP)的流量设置了有两个 Threshold: Maximun Threshold, Minimun threshold. 当流量低于最小阈值时, 不丢包. 当流量超过最小阈值, 低于最大阈值时, 随机丢包. 当流量大于最大阈值时, 队列即将占满, 全部丢包. 因为每一个分类的阈值不同, 可以保证每个分类的最小带宽. 一般情况下, 会把 FTP 的最大阈值设置得很低, 这样当语音流量来的时候, 我们可以预留一定的 buffer

# 丢包
1. 尾部丢弃tail drop 队列满了丢弃
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

Interactive 交互式的, 不需要大吞吐量, delay 也不高, loss, 
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

DiffSe



# 二层 QoS
COS
二层, 101=5, 110=6 表示语音

# 三层 QoS
在 ip 包头中
TOS = Type of Service. 8bit
```shell
|  DiffServ |
|1|0|1|0|0|0|ECN|ECN|
```

早年只使用了前三位称为  IP Precedence. 000 = Best Effort
但后来不够用了, 就扩充到 6 位, 称为 DSCP Difference Service Code Point
DSCP 是向前兼容IP Precedence 的

DSCP 的结构
在 DSCP 中, 前 3 位为Class-Selector 或称为 CS value 
前三位的表示

|Precedence Level |Description|comments|
|---|---|---|
|7|Stay the same (link layer and routing protocol keep alive)|keepalive报文使用, 优先级最高|
|6|Stay the same (used for ip routing protocol)||
|5|Express Forwarding(EF) |快速转发 premium service, 专用于 voice |
|4|class 4||
|3|class 3||
|2|class 2||
|1|class 1||
|0|Best Effort||


如果 4-6 位非 0, 则把前 6 位称为 AF Assured Forwarding  
AF 有 4 类, 分别是 AF1, AF2, AF3, AF4
AF1 = 001, AF2 = 010, AF3 = 011, AF4 = 100
AF 位的大小表示包的重要性, AF 位越大, 重要性越高
比如AF4的重要性大于 AF1

AF的每一类内, 4-5 位有 3 个分类
比如 AF1 有下面 3 个分类, 注意最后 1 位, 没有使用
001 010 = AF1 1 = DSCP 10
001 100 = AF1 2 = DSCP 12
001 110 = AF 1 3 = DSCP 14
这里 4-5 位称为 dd 位, 表示包被丢弃可能性, 值越大, 在当前 AF 类中更容易被丢弃
比如当拥塞时AF13的丢包概率大于 AF11

Per hop behavior 每一跳行为
4 类
1. Default, Best Effort
2. Class-Selector  DSCP 的高 3 为 IP 优先级, 后 3 位为 0. 
3. Expedited forwarding  EF位101 110 = DSCP 46 快速转发, 用于语音 
4. Assured Forwarding  AF保证转发, 会做, 但是要等. 会有管制流量, 超过的会被丢弃

因为 IP 优先级有 3 位, 所以一共定义了 8 种优先级
0-routine
1-priority 数据data 优先
2-immediate 数据
3-flash 如果把 3 映射为 DSCP 一般是 24, 乘 8 的关系
4-flash-overide 急速,用于 video
5-critical  voice 使用 101
6-internet  分配给路由协议来使用
7-network 分配给路由协议来使用




AF43 丢弃可能性大于 AF21, 因为 3>1 
拥塞避免 Congestion avoidance 针对不同的 AF 有不同的待遇



# 实验 
* QoS 配置是有方向的, 下面的实验是在一个方向的, 反方向没有配置
* 

[视频参考](https://www.udemy.com/course/complete-networking-fundamentals-course-ccna-start/learn/lecture/18293214#overview)
1. 用 class-map 抓取流量
2. 用 policy-map 做标记, 比如 EF, AF31等. 
3. 在接口下应用 policy-map, 指定方向

```shell
# R1
# 1. create class map to match the traffic
class-map match-all voice
match protocol rtp  # real time protocol 
exit

class-map http
match protocol http

class-map icmp
match protocl icmp

# 2. Create policy map to mark the traffic
policy-map mark
class voice # refer class-map voice created above
set ip dscp ef
priority 100
exit
class http # refer class-map http created above
set ip dscp af31
bandwidth 50 # minimun bandwidth
exit
class icmp
set ip dscp af11
bandwidth 25
exit
exit

# 3. apply policy map under interface
int serial 0/1/0
service-policy output mark # egress, refer policy-map created above

# make http or icmp request
show policy-map # 查看配置的 policy
show policy-map int s0/1/0  # 查看接口下class-map实际上抓取的流量
```

R2
```shell
# match the traffic marking from R1
class-map match-all voice
match ip dscp ef
class-map match-all http
match ip dscp af31
class-map match-all icmp
match ip dscp af11

# use policy to remark the traffic
policy-map remark
class voice
set precedence 5
exit
class http
set precedence 3
exit
class icmp
set precedence 0 # best effort
!

int s0/2/0
service-policy input remark 
# 通过 packet tracer simulation 模式可以看到 DSCP 值在 inbound 和 outbound 变动
```


# Link Specific Tools
Link Fragmentation and interleaving 不在 CCNA 范围内
