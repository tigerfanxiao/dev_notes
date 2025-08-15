ICMP 报文长度, VID 多少字节

Ping和Tracert都是基于ICMP协议

# Ping
Ping有两种报文：
1. Echo Request 像对方主机请求
2. Echo Reply 对方主机的回复

### 错误提示 Destination host unreacheable
目标设备不在一个广播域中， 而且在主机上也没有配置网关， 找不到网关

### 错误提示 Request Timeout
Echo Request 发了， 但是没有回复， 请求超时


# Tracert
Trascert主要是看目标设备向发起方发Echo Reply的源IP地址。 