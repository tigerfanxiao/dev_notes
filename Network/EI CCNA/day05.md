
### 动态路由协议
动态路由协议和静态路由的区别在于, 动态路由协议不是实现规划好的路线, 而是挺邻居说的, 自己考虑的, 最后决定走的路. 当然邻居提供的方向也不一定对. 可能遇到路被堵了. 动态则体现在, 我们还能根据邻居提供的更新信息, 动态调整路线

可以看出动态路由协议有几个关键的点
1. 了解相邻路由器的子网路由信息. 每个邻居都宣告自己直连的网段 - 学习(学过来)
2. 把路由信息发布给相邻的邻接 - 更新 (发出去)
3. 在多个路径中根据度量标准寻找最佳路径 - 选路(我觉得)
4. 根据故障和其他情况自动调整路径 - 收敛 (见机行事)

OSPF 值. 用 100M 除以出接口带宽 Mbps
为什么要分区域, 主要是一个区域所有的路由器都要把自己的网段信息发出来. 如果一个区域中包含很多路由器, 则每个路由器都需要自己算最优路径. 很多路由器性能达不到的话, 就跑不起来. 而且任何拓扑变更, 都要导致整个区域的路由器重新计算

1. 较大的拓扑数据在每个路由器上需要更多的内存
2. SPF 算法要求的处理能力与拓扑数据库的大小成指数级增长
3. 任何位置发生网络变化, 将强制路由器再次进行 SPF 计算. 哪怕是接口 down 了, 或者加了新的接口进来
OSPF 必须要有区域0, 所有别的区域都要挂到区域 0 上. NP 阶段会介绍虚链路技术, 将其他区域不直接挂到区域 0 上

OSPF 进程号, 是本地的. 一台设备上可以起多个进程. 同一个设备不同的进程之间不共享. router-id 必须全网唯一
特殊场景. 在防火墙内外, 使用两个 ospf 进程. 使内网的 ospf 不会把信息同步到外网的 OSPF 中
### VPN
了解 VPN 的封装思想
