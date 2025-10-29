# Reset Factory Mode
锐捷交换机的配置是保存在 `config.text`中的
```shell

# 手动断电重启设备
# Ctrl + C 进入 bootloader 菜单
# Ctrl + Q，进入uboot命令行
# 输入下面命令, 清楚console 密码
main_config_password_clear
Ctrl + C


help
# 重命名现有的配置文件, 这步操作后, 重启设备会丢失现有配置
rename config.text config.text.bak
# 重启
load

# 重启完成后
enable
copy config.text.bak config.text # 恢复配置文件 

# 如果以上命令报错，则使用以下命令恢复配置文件：
copy flash:config.bak flash:config.text

# 使配置生效
copy startup-config running-config
```
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


### 出口限源
```shell
line vty 0 4
 transport input ssh
 access-class For_Access_Restrictions in
 exec-timeout 180 0
 privilege level 15
 Login local
!
end


ip access-list standard For_Access_Restrictions
 10 permit host 30.118.110.91 
 20 permit host 30.118.110.92 
 30 permit host 30.118.110.93 
 90 permit host 30.118.110.179 
 100 deny any

```