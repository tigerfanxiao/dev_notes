
### SD-WAN 产品
SD-WAN 第一象限企业是 Velocloud, 目前被 vmware 收购
思科的 SD-WAN产品叫 Viptela

SD-WAN的特点是利用廉价的互联网带宽达到之前只有企业专线一般是 MPLS 专线才能达到的带宽和稳定性. 同时可以实现零接触部署. 灵活连接各种公有云和 SaaS 服务. 

MPLS 的问题
MPLS 首先是贵, 第二是不灵活, 部署起来很慢. 首先运营商要铺设专门的光缆到指定地区. 然后需要在运营商的 PE 设备上做配置. 同时分支机构的 CE 设备, 也需要专业人员配置. 

SD-WAN 可以将专线, 互联网链路, 5G 链路抽象出来, 形成一个 overlay 的网络, 实时测量链路的拥塞, 抖动, 延迟. 可以基于应用对网络性能的需求, 分配流量到不同的链路上. 甚至单挑链路的上行链路断了, 还能借助别的链路来传输上行流量, 大大增加了网络的可靠性. 

SD-WAN 可以是敏捷和与硬件解耦的, 因为 SD-WAN 可以开放北向接口用于编程, 所以可以通过联通其他监控软件对链路和应用的流量做实时的监控和报告生成. SD-WAN 的南向接口一般用的是每个厂家的私有协议
### QoS
SD-WAN 可以基于应用做动态 QoS有效调优. 原来 QoS 只能基于 IP, 网络, 端口. 且原来 QoS 需要在每一台设备上做调整. 工作量巨大. 


# SD-WAN 的部署
SD-WAN是 SDN 概念下的产物. 核心的概念是将传统网络设备的中的控制面和数据转发层面分离开. 将控制层面抽象出来, 用一个控制面去管理其他设备的数据面. 
控制面可以部署在指定的设备上, 也可以部署在通用芯片服务器上, 或者部署在云端. 

在华为的方案中, SD-WAN是部署在服务器上, 在服务器上安装 NCE-SD-WAN 作为控制器, FusionCompute 来建立资源池. 所以 SD-WAN 也是一种 NFV 技术, 可以虚拟出交换机作为数据面. 
同时 FusionCompute 也会实时探测链路的负载程度, 完成亚秒级的链路切换.

当需要扩容时, 直接把设备接入控制器, 在控制面选择相应的角色, 则会自动完成部署
### 传统的企业WAN 方法
MPLS VPN 也是运营商说的企业专线. 特点是稳定, 上下行链路一直, 但是价格昂贵
IP Sec VPN 一般是企业内部部署, 承载在普通的互联网链路上. 能保障安全性, 但是抖动和延时不能保障
传统企业的解决方案, 一般是用 MPLS 作为主要公网链路. 而 IP Sec VPN 作为备用链路

### 传统企业 WAN 架构的问题
1. 很难基于应用来选择链路. 比如视频会议这类应用对网络的延迟要求很高. 有些公司通过 SLA-EEM 的方案监听每个应用消耗的资源, 来做策略. 这种配置相当复杂
2. 分支机构如果要使用公有云, 或者访问公网 SaaS 服务往往需要 backhual 回传到总部, 经过总部的防火墙策略后, 在访问公网. 而不是直接访问公网. 这样会占用昂贵的专线带宽.
3. 如果分支机构有一部分流量需要访问总部, 一部分流量需要访问互联网, 则对应的策略也比较难做.

### SASE 架构
在 SD-WAN 中可以插入各种网络服务, 其中也包括安全服务. 可以理解为在公有云上部署了 SD-WAN, 然后里面插入了安全服务. 分支的流量就回传backhaul 到总部后再去往公网. 而是直接连接到公有云上的 SD-WAN, 经过安全服务就能访问公网. 而分支结构也不需要额外购置防火墙设备了. 因为安全策略可以统一在 SD-WAN 中的安全服务中做

### SD-WAN 的经营模式
SD-WAN 支持多租户的. Velocloud是卖给运营商. 然后由运营商贴牌卖给企业. 

### SD-WAN的应用
- 一般手机有 4,5G 信号和 wifi 型号. 手机上也可以安装 SD-WAN 人软件实现两个链路的切换和负载分担
- SD-WAN 可以做为 IoT 设备的网关





