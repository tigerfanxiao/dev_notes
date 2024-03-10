
连接防火墙或者路由器的上行接口

```shell

interface GigabitEthernet 0/47
 poe enable
 switchport access vlan 1000
 description TO-FW

```

问题： 怎么做 `eth-trunk`


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
