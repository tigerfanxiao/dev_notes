# 进度

武汉誉天
一共 184 个视频



# 进度
0-4

### 进制转换

16 进制 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

4 个 bit 表示一个 16 进制为
2个 16 进制位= 8个 bit, 所以一个字节是 2 个 16 进制位表示的
mac 地址是 48bit, 6 个字节, 6 个 16 进制位





### 什么是路由
* 路由是指导IP报文发送的路径信息
* 路由协议是路由器之间交流的语言。
路由信息的获取来源
* 直连接口，从down到up状态的过程，是链路层自己完成的。也成为直连路由
* 静态方式，网络管理人员手动配置
* 动态路由协议

### 静态路由和动态路由对比
* 静态路由需要手工配置和维护。动态路由可以自动适配拓扑的变化

### 交换机和广播域
当交换机收到广播后， 会将广播报文复制成若干份，并从其他接口发送出去。 
广播域： 收到同一份广播报文的终端组成的网络

### 什么是网关
网关是一种相对的概念， 是指当前设备去往目的网络的下一跳地址。


### 路由器的作用
* 跨广播域传输数据
* 检查ip报文的目的地，查找路由表
* 选择最佳路由
* 维护路由信息， 因为路由信息可能失效
* 发现可能的路由
* 单播和组播可以经过路由器传输
路由器的一个接口都是一个独立的广播域。



路由协议的分类
按照范围分类： IGP vs EGP
根据算法分类： 距离矢量路由协议 vs 链路状态路由协议
距离矢量： RIP， BGP
根据路由协议的业务分类： 单播 vs 组播
单薄： 一对一的通信 RIP， OSPF， BGP,  IS-IS
组播： DVMRP， PIM-SM， PIM-DM


路由表
1. 最长掩码匹配原则
2. 按照路由协议的优先级
3. 按照cost的优先级
4. 如果cost相同， 负载分担

路由环路
1. TTL变为0

协议优先级
RIP 100
OSPF 10
BGP 255
Static 60
静态路由 0


# 静态路由
静态路由不能修改cost， 如果要做主备，可以修改preference来实现

# 缺省路由
如果匹配不到具体某一条路由信息， 会使用缺省路由，一般指向网关
```shell

ip route-static 0.0.0.0 0.0.0.0 1.1.1.1 # 1.1.1.1 为下一跳地址
ip route-static 0.0.0.0 0.0.0.0 2.2.2.2  # 实现负载分担
```


自治系统 AS： 由同一个技术管理机构管理的，使用统一选路策略的一些路由集合

# 路由引入
不同的路由协议之间用 `import-route`来实现交换路由信息

对路由协议的衡量
* 正确性： 能够找到最优的路径，且无自环
* 快收敛： 当网络拓扑发生变化时，能够迅速在自治系统中做相应的路由改变
* 低开销:  协议自身的开销（内存，CPU，网络带宽）最小
	* 在做割接操作时，要注意device中的cpu利用率，因为isis会重新选路
* 安全性：协议自身不容易受到攻击，有安全机制
* 普适性： 适应各种拓扑结构和各种规模的网络

### 堆叠
在金融和数据中心场景上, 目前很少使用堆叠技术, 而是更多的使用 m-lag 技术. 主要堆叠的技术虽然在理论上可以堆叠 5-9 台, 但是在实际使用中, 堆叠设备中的master 设备随着堆叠设备数量的增加, CPU 负载会很快增加. 可能会因为master 设备 CPU 过载而出现事故. 但是一些对带宽和速率要求不那么高的场景, 比如医院和学校, 堆叠技术在现网中广泛应用. 

不同的行业对于安全性和容灾的需求也不一样. 金融行业对于数据容灾的需求特别高, 而对于成本的敏感性相对会低一些. 

在 CCIE 的认证中, 对于运营商和数据中心的认证是分开的, 可见不同的行业使用的技术选型也不尽相同

# 无线
wifi6理论上可以达到10G



# 防火墙
作用
* 隔离不同安全级别的网络
* 实现不同安全级别的网络之间的访问控制(安全策略)
* 用户身份认证
* 实现数据加密及虚拟专用网业务
* 执行网络地址转换
* 其他安全功能 IPS

IPS 入侵防御检测功能