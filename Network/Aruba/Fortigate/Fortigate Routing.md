Fortigate 上可以 OSPF, RIP, BGP, 也可以做路由重分布
```shell
# 配置一个接口

config system global
set hostname FW
end
config system interface
set ip 202.100.2.30 255.255.255.0
set allowaccess ping http ssh https

config router ospf
set router-id 3.3.3.3
config area
edit 0.0.0.0
next
config network
set prefix 202.100.2.0 255.255.255.0
set area 0.0.0.0
next
end

# 查询 OSPF 邻居
get router info ospf neighbor
get routter info routing-table all

```
在图形界面上配置
![[Pasted image 20240728190649.png]]
VIP 也称为 Virtual IP, 用于外网访问内网. 一般做的是一对一的转换, 然后做端口映射. 
比如外部一个终端要telnet内网的服务器. telnet 一个公网地址, 端口号为 2323. 防火墙将这个流量映射内网 DMZ 区域的服务器上, 将端口 2323 映射为 23

这种技术也称作为 DNAT, 即 Destination NAT. 相对应的如果是从内网出去做地址转换, 称作SNAT 即Source NAT

下一代防火墙, 引入基于对象的策略
单个 IP 地址是一个对象, 一个 IP 地址段时一个对象, 一个 zone 也是一个对象. 
基于不同对象, 或者多个对象可以做策略


FQDN 是域名, 可以控制访问哪些域名
可以控制每个IP地址可以分到的带宽

Central NAT 貌似是在一个集中的地方做 SNAT

宽带测速工具 Angry IP Scanner 可以看到内网中的所有终端, 可以用来扫描服务器. 因为服务器一般是开放 80 端口允许页面访问

1. 防火墙可以控制大量下载
2. 每个用户的最大并发数量

在配置静态 NAT 时候, 防火墙默认是否是支持代理 ARP 的? 可能需要手工激活. 因为我们也没法查看ISP网关的 ARP 表


接口下 dhcp 配置方法

```shell
config system dhcp server 
    edit 1  
        set dns-service default  
        set default-gateway 192.168.1.1  
        set netmask 255.255.255.0  
        set interface "port1"  
            **config ip-range**  
                edit 1  
                    set start-ip 192.168.1.2  
                    set end-ip 192.168.1.254  
                next  
            end  
    next  
end
```