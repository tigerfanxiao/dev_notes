

# 命令
```shell
# 查看所有接口配置
show system interface

edit port6
 set vdom root # 类似于 vrf, 这里默认没有 vrf 的话就是 root
 set type physical
 set snmp-index 6

# 查看单个接口配置
show system interface port1
```


配置命令
```shell
config system interface # 进入接口配置
 edit port1 # 在数字的地方可以 tab
 set mode static # 配置静态 IP 地址, 默认是 DHCP
 set ip 10.1.1.20 255.255.255.0
 set allowaccess ping # 允许被ping. 默认是允许 ping 别人的
 next
end


# 测试直连
execute ping 10.1.1.1
```

注意: 华为设备 ping 别人也要放行. Fortigate 默认可以 ping 别人

打授权一定要能够上公网

```shell
config router static # 配置静态路由
edit 1
set device port 4 # 4 号接口
set dst 0.0.0.0 0.0.0.0
set gateway 202.100.1.1
next 
end
# 查看路由表
get router info routing-table all 

# 查看静态路由配置
show router static


```

HA
为什么热备一定要用一套 IP 和 MAC?
因为如果用两套 IP 和 MAC, 在一个设备挂掉的时候, 在交换机中记录的是挂掉的设备的 MAC 地址. 除非启动的另一台冷备设备发送免费的 ARP, 来刷新交换机中的 ARP 表, 否则交换机还是不断的给挂掉的设备发送流量

在传统的技术中, HRSP 就是用虚拟的 IP 和虚拟的 MAC

