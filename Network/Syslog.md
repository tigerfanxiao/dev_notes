
每个设备都会吐 syslog, 但是 syslog 的存储方式有很多. 一般情况下,每个设备上的 syslog 是保存在设备的内存中, 一旦断电也就丢失了. 
像防火墙这样的设备一般会配置一个 ssd 卡, 用来存储 syslog, 这样断电也不会丢失. 
更复杂一些的是, 厂商有专门存储syslog 的服务器和分析 syslog 的产品. 比如 fortinet 就有

Syslog 本身也是一种标准协议. 它包含的信息有: 设备的重启, OSPF 的邻居的 up 和 down

syslog 的格式
![[Pasted image 20240322061550.png]]

syslog 的严重程度
Every Awesome Cisco Engineer Will Need Ice Cream Daily
![[Pasted image 20240322062033.png]]


```shell
# 在telnet和ssh连入下, 查看 syslog, 默认情况是不显示的
show syslog

# 在console 输出 6以上的syslog, debug 不输出
logging console 6 
# vty 输出
logging monitor informational 
terminal monitor # 必须开启后才会显示, 且是基于会话的
# buffer 输出, 一次满 8192 个字节就输出
logging buffered 8192 6
# 输出到服务器
logging 192.168.1.100
logging host 192.168.1.100 # 和上面的命令一样
logging trap debugging # debugging 以上的都发送给 logging server
```

syslog 存储的地方
1. console 输出
2. vty 输出
3. buffer 输出, RAM 输出
4. syslog 服务器输出
注意: 默认情况下 syslog 的输出地方是 console 和 buffer

syslog 服务器会监听 udp 514 端口来接受 syslog

注意: 默认情况下, 思科设备在 ssh 或者 telnet 接入时是不显示的, 即使`logging monitor informational` 已经配置了. 必须开启 `terminal monitor` 才能卡看到, 且每次会话都需要重新开启. 原因是这样不会在你做配置的时候, 突然显示 syslog. 
```shell
# 对 console 显示进行配置, syslog 会重启一行
line console 0 
logging synchronous

# timestamps 和 datetime 是否显示是可以配置的
service timestamps log datetime # 事件发生的时间

# 以下两个配置很少使用
service timestamps log uptime # 从设备启动到事件发生的时间
service sequence-numbers # 显示序列号, 意义不大
```
