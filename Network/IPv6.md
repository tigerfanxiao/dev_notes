
IPv6 地址长度 128 为, 是 IPv6 的 4 倍

无需 NAT
不再有广播 ARP, 用 ICMPv6替代
IPv6 的报头比 IPv4 更有效率, 字段更少, 去掉了头部的校验和
新扩展的头部取消了 Option 字段, 



IPv6 到 IPv4 的过渡方式

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
