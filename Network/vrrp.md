VRRP的组播是地址224.0.0.18. 作用是在两个vrrp设备之间建立通信, 获得对方的心跳. 
但是VRRP不能感知上方联络是否中断


VRRP的主要目的是防止单点故障. 一般是为下行的联络提供出口网关
将两个网关, 虚拟成一个网关
可以在物理接口下配置, 也可以在vlanif下配置. 但是在接口地下需要有IP地址
默认的接口vrrp优先级是100, 优先级范围是0-254. 越高越优先
如果设备挂掉, 设备的优先级会下降10
同一时间是有master设备在转发数据

### VRRP基本配置

```shell
# master 
interface G0/0/0
ip address 192.168.1.3
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 1 priority 105

# backup
interface G0/0/0
ip address 192.168.1.4
vrrp vrid 1 virtual-ip 192.168.1.254 # 在同一个vrid组里

```

特性配置
```shell
 vrrp vrid 1 preempt-mode timer delay 120 # 抢占延迟120秒
 
 vrrp vrid 1 track nqa admin wan reduced 40
```

### 查看命令
```shell
display vrrp verbose 
```

# VRRP和NQA联动
