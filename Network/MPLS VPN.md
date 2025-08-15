

MPLS VPN 压了两层标签. 最外层的标签(LDP 标签)由 LDP 协议分发, 用于在 MPLS 域内建立 LSP. 内层标签(VPN标签)由 MP-BGP 协议分发, 分配给同一个 vrf 下不同的路由前缀


```shell
show ip bgp vpnv4 all labels # 查看vpnv4 标签

clear ip bgp vpnv4 unicast 234 soft out # 刷新vpnv4邻接关系
```