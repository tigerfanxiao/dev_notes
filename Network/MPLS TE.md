[B 站视频材料](https://www.bilibili.com/video/BV11B4y117r7/?spm_id_from=333.337.search-card.all.click&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
MPLS TE 也称为流量工程 Traffic Engineer. 在 MPLS LDP 的技术中, 因为 LDP 本身没有路由选路的能力. 标签的分发, 或者说 LSP 的建立是基于下层 IGP 协议完成的. 也就是说底层的 OSPF 或者 IS-IS 协议都是基于 SPF 算法, 基于开销值 cost 算出最短的路径. LSP便只能选择这条路径来转发流量. 

基于 LDP构建 LSP会造成一个问题. 就是没有办法实现链路的冗余备份和负载分担. 无法充分利用运营商网络中的带宽资源. 所有的流量都集中在一条 LSP 上. 容易造成拥塞. 

流量工程就是为了解决上面的问题开发出来的. 我们已经知道 LDP 是问题的结症所在, 所以在 MPLS TE 中, 我们用 RSVP 来取代 LDP 来构建 LSP. 

此外, 因为我们需要多条链路, 而且需要具备基于带宽的选路能力, 我们需要对下层 IGP 协议进行扩展, 在 LSA 中发送接口带宽信息. 由这些信息构建的数据库TE DB, 来选路. 那么这种以带宽为依据的选路算法, 就称为 CSPF 即 Conditional SPF.

RSVP Resource Reservation Protocol 的作用 
1. 标签分发, 构建 LSP
2. 发送信令, 向沿途节点盛情预留资源(带宽)

流量工程中的选路, 可以基于带宽自己选路, 也可以手动指定路径. 

# MPLS TE组件
1. 信息发布组件  IS-IS,  OSPF
2. 路径计算组件 
3. 信令组件 RSVP
4. 报文转发组件

IS-IS 扩展
通过 TLV 扩展

MPLS VPN 默认是不能承载在RSVP 上的, 而是承载在 LDP 上的. 所以在 MPLS RSVP 的 TE 中, 如果要使用 MPLS VPN, 需要在 tunnel 中使用 policy

