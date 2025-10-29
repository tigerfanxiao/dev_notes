
很多时候, 你拿到一个项目的时候, 没有网络拓扑图
想要知道网络设备在物理上是怎么连线的, 就可以通过LLDP网络发现协议来

### 原理
每一台设备上都有MIB. MIB里面存储了设备的管理信息, 包括IP地址, 设备型号等. 
设备之间通过互相访问MIB, 了解对方的信息
### 场景

### NMS场景
NMS network management system 网关系统. 当网络中有网管系统时, 可以通过LLDP协议知道网络中所有设备的拓扑结构

#### LACP 场景
当两台设备之间有链路聚合之后. 在overlay网络看, 两台设备只有一条链路. 在underlay网络看, 两个设备之间有多条物理连线. LLDP只能查看物理连接. 所以可以看到多个接口互联

```shell

lldp enable # 开启lldp
display lldp neighbour # 
```