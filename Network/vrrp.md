VRRP的组播是地址**224.0.0.18**. 作用是在两个vrrp设备之间建立通信, 获得对方的心跳. 
但是VRRP不能感知上方联络是否中断, 所以一般使用NQA或者BFD技术探测北向链路是否中断

# 理论
master会每隔1秒发送通告报文给Slave自己还活着. 如果3秒内slave没有收到master的通告报文, Slave就会任务自己是master
抢占: 当master恢复通信后, 会重新把master从slave那边抢过来. 



VRRP的主要目的是防止单点故障. 一般是为下行的联络提供出口网关
将两个网关, 虚拟成一个网关. 同时为这个网关构造一个虚拟的mac地址. 
**0000-5e00-开头**
可以在物理接口下配置, 也可以在vlanif下配置. 但是在接口地下需要有IP地址
**默认的接口vrrp优先级是100**, 优先级范围是0-254. 越高越优先. 0和255是保留的. 如果优先级相同, 则比较接口IP地址的大小, 大的优先

Master是负责流量转发的设备. Slave上没有流量, 并没有负载


如果设备挂掉, 设备的优先级会下降10
同一时间是有master设备在转发数据

### VRRP基本配置
基本配置只能保证两两个网关合并起来, 但是不能做到抢占

```shell
# master device
interface G0/0/0
ip address 192.168.1.3
vrrp vrid 1 virtual-ip 192.168.1.254  # 192.168.1.254是续集的地址
vrrp vrid 1 priority 105
# 如果主设备的某个接口挂了, 则会降低优先级. 备设备抢占
vrrp vrid 1 track interface g0/0/0 reduce 10 # 如果接口挂了, 优先级降低10
vrrp vrid 1 preempt-mode timer delay 120 # 抢占延迟120秒. 就是master恢复后120秒再抢占回来
# backup device
interface G0/0/0
ip address 192.168.1.4
vrrp vrid 1 virtual-ip 192.168.1.254 # 在同一个vrid组里

```

### 查看命令

```shell
display vrrp verbose 
display vrrp 
### 这是在backup设备上看到的回显
[SW2]display  vrrp
  Vlanif10 | Virtual Router 10
    State : Backup
    Virtual IP : 192.168.1.254
    Master IP : 192.168.1.253
    PriorityRun : 100 # 当前的优先级, 如果reduce从这里可以看出
    PriorityConfig : 100
    MasterPriority : 101
    Preempt : YES   Delay Time : 0 s
    TimerRun : 1 s
    TimerConfig : 1 s
    Auth type : NONE
    Virtual MAC : 0000-5e00-010a  # 除了虚拟的ip之外, 还有virtual mac
    Check TTL : YES
    Config type : normal-vrrp
    Create time : 2023-09-30 19:00:41 UTC-08:00
    Last change time : 2023-09-30 19:05:03 UTC-08:00
    
# 可以看到当前设备是主还是备
display vrrp brief
[SW1-GigabitEthernet0/0/22]dis vrrp brief
VRID  State        Interface                Type     Virtual IP     
----------------------------------------------------------------
10    Backup       Vlanif10                 Normal   192.168.1.254  
----------------------------------------------------------------
Total:1     Master:0     Backup:1     Non-active:0 
```

# VRRP和NQA联动
为什么要用NQA? 
如果是直连链路, 本端接口或者对端接口挂了, VRRP都能检测出来. 但是现实中, 出口路由器到运营商网络时, 可能中间还有多台2层设备. 如果不是和我本地的直连链路挂了, 而是运营商侧的某个二层设备的链路挂了. 我们就无法感知. 这是就要有3层协议NQA或者BFD来监控链路

```shell
nqa test-instance admin wan
test-type icmp
detination-address ipv4 172.16.11.2
frequnce 1 # 每一秒测试一下
timeout 1
start now  # 立即启用

# vrrp调用nqa
int g0/0/0
vrrp vrid 1 track nqa admin wan reduced 10

```

# VRRP和BFD联动

```shell
bfd # 全局开启
quit
# 配置单臂回声
bdf 2to1 bind peer-ip 172.16.11.2 interface 0/0/22 one-arm-echo
discriminator local 10 # 这是bfd的标识
commit # 必须加commit才能保存成功

int g0/0/0
vrrp vrid 1 track bfd-session 10 reduced 10
```

