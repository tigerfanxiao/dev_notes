Board Gate Protocol 

BGP 和 IGP 的区别
1. BGP 是 AS 之间使用的路由协议. IGP 是 AS 内部使用的路由协议. 
2. BGP 邻居的建立需要 TCP 连接来维持

BGP邻居建立状态


选路原则 8 条
注意: 不同于 OSPF, 带宽不在选路原则里
We Love Oranges As Oranges Mean Pure Refreshment
1. Weight   这个特性是思科私有的, 不能传到其他 AS
2. Local Preferance 公用属性, 可以在 AS 中传播, 用于修改出去往哪里走
3. Originate 直连的网络优先
4. As Path Length 越短越优先, 多用于修改进来往哪里走
5. Origin Type 通过 network 命令宣告的优于重分布进来的
6. Multi-Exit Discriminator MED 值越小越好
7. Path  EBGP路径优于 IBGP 路径
8. Router ID 越小越优先

EBGP 和 IBGP 邻居
EBGP邻居建立, B 从 A 邻居学到的路由, 会传递给 C
IBGP 邻居之间, B 从 A 邻居学到的路由, 不会传递为 C. 默认是期待在 AS 内部, 所有的 IBGP 邻居形成一种全互联的 mesh 的邻居关系. 也就说, A 的路由要直接传递给剩余的 n-1 个邻居. 但是这种全 mesh 的结构不适合 scale. 所以才有了 Router Reflector 技术. 这个技术解决了两个问题. 
1. 解决了全 mesh 的结构, 而是类似 OSPF 中 DR/BDR 的模式, 构建成星形的结构, 这种方式称为路由反射器RR. 还有一种方式是将 AS 中的设备拆分成多个联邦 Confederation
2. 因为 A 传给 B的路由, B 不能传给 C. 有了路由反射器之后, A 直接把路由交给 RR, RR 负责把路由反射给剩下的 n-1个邻居. 
这里出现了一个问题, 就是剩下的 n-1个邻接学到是A 从对面 AS 的路由, 他们自己不是EBGP 的边界路由器, 不知道怎么去往对面的 AS. 这是造成BGP 学到了路由, 但是下一跳不可达的情况. 要解决这个情况, 就需要再边界路由器上告诉自己的IBGP 邻接, 下一跳是自己. 


BGP 的放环机制
* EBGP 发现有自己的 AS 号, 直接丢弃
* IBGP 水平分割

## BGP 路由黑洞问题
IBGP水平分割规则和BGP可以跨过直连设备建立邻居关系的特点，造成了BGP路由黑洞问题。所谓BGP路由黑洞问题，实际上就是BGP在路由传递过程中跨过中间路由器，从而导致了在数据包发送的时候中间的路由器无法收到路由信息，从而导致数据包转发失败的现象。

![[Pasted image 20230416232210.png]]

1. R1和 R2 是 EBGP 邻居. R1 宣告了一条 1.1.1.1 路由, 传递给 R2
2. R2和 R4 是 IBGP 邻居, R2收到 1.1.1.1 路由后, 传给 R4, 指定下一跳是自己. 
3. 注意: 这里 R3没有跑 BGP
4. R4学到1.1.1.1 后又传给了 R5. 
5. 当 R5要访问 1.1.1.1时, 流量走到 R4. R4的查询路由表, 下一跳是R2. 但是因为 R2和 R4没有直连, R4只能查询IGP路由表, 发现要到R2需要经过R3.
6. 流量到达R3时, R3收到包, 查看目的地址是 1.1.1.1, 但是 R3 没有跑 BGP, 没有学到 1.1.1.1的路由, 所以直接丢弃. 此时就产生了路由黑洞

## Multihop 
[参考](https://orhanergun.net/ebgp-multihop#:~:text=BGP%20is%20also%20used%20for,flexibility%20in%20inter%2Ddomain%20routing.)



# IPv6 BGP

1. 手动激活邻居
2. 用route-map手动指定下一跳是自己的接口

