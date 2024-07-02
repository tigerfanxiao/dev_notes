# 恢复出厂设置
断电重启设备
```shell

Ctrl + C

help
rename config.text config.text.bak
# 重启
load

```


连接防火墙或者路由器的上行接口



### L2 ACCESS

```shell

interface GigabitEthernet 0/47
 poe enable
 switchport access vlan 1000
 description TO-FW

```


### Vlanif
```shell
interface Vlanif300
 description wired-office
 ip address 10.240.5.1 255.255.255.0
```


### Hostname
```shell
hostname CSW-SPAIN-A.SPAIN-C
```
### LACP

二层链路绑定
```shell
int GigabitEthernet 0/47
 description To_ASW_SPAIN
 port-group 1 mode active
!
int GigabitEthernet 0/48
 description To_ASW_SPAIN
 port-group 1 mode active
!

interface aggregateport 1
switchport mode trunk
exit

show aggregate summary
show interface aggregateport 1


```


### NAT

```shell
# 公网出口
int teng 0/51
ip add 10.36.1.9/24
ip natoutside

```





用 hybrid 方式来配置 access 口， 用于内网 PoE 设备，比如影音设备
在办公室的 RJ45 接口很可能被私连交换机，或者线路错连成为环路， 所以在接口下要配置防止网络风暴的 PPS Packet Per Second
```shell
interface GigabitEthernet 0/37
 poe enable
 switchport mode hybrid
 switchport hybrid native vlan 100
 storm-control broadcast pps 500 # 广播
 storm-control multicast pps 500 # 组播
 storm-control unicast pps 1000 # 单播
```

问题： 最佳实践 pps 怎么计算
