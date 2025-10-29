
NAT 做私网地址和公网地址的转换
NAT可以做到源地址和目标地址同时转换, 也称为双向NAT



当回来的流量到达NAT节点时, 会被抓取, 将公网的目的IP地址(配置在出口路由器上的ip地址) 替换为私网的ip地址


NAT的分类
1. 让私网用户访问公网, 称为SNAT. 因为会变更流量的源地址. 华为提供了两种方式
	1. 地址池 需要运营商提供公网地址池
	2. Easy IP 只要一个公网地址
2. 让公网用户访问私网的资源, 也称为NAT Server

静态NAT
每一个私网地址对应一个公网地址. 无法节省公网地址
使用场景: 多用于内网服务器的映射. 不会用在主机上

PAT
做端口地址转换, No-PAT不做端口地址转换

# 动态NAT
找一组公网IP作为地址池
问题是, 如果同时上网的主机多余地址池数量的话, 就不行了


NAPT 网络地址端口访问
用ACL把内网流量一起通过某个接口转换

```shell
# 定义一个ACL抓取内网流量
acl 2000
rule 5 permit source 192.168.1.0 0.0.0.255

int g0/0/0
ip add 12.1.1.1 24  # 外网IP
nat outbound 2000
```

### easy IP 
不用地址池了, 就用一个公网地址, 进行一对一的映射
瓶颈是并发端口超过65535

用ip地址配置
```shell
# 把公网100.1.1.1地址, 映射到内网192.168.1.1
nat server global 100.1.1.1 inside 192.168.1.1 

```

**在接口下通过抓取流量来配置** 
这种方法不再局限于单个流量入口
```shell
# 通过一个acl把需要出去的流量抓出来

acl number 2001  
 rule 5 permit source 192.168.10.0 0.0.0.255 
 rule 10 permit source 192.168.11.0 0.0.0.255 
 rule 15 permit source 192.168.20.0 0.0.0.255 
 rule 20 permit source 192.168.30.0 0.0.0.255 
#

interface g0/0/7
undo portswitch
description TO_INET
ip add 178.19.44.122 255.255.255.248  # /29 的公网ip地址
nat outbound 2001 # 把 Acl 2001的流量做nat地址转换

# 运营商一般要求调整速率
undo negotiation auto
speed 1000

```


### 查看NAT状态
```shell
<AR_backup>dis nat outbound 
 NAT Outbound Information:
 --------------------------------------------------------------------------
 Interface                     Acl     Address-group/IP/Interface      Type
 --------------------------------------------------------------------------
 GigabitEthernet0/0/7         2001                        0.0.0.0    easyip  
 --------------------------------------------------------------------------
  Total : 1
```

我们的家庭宽带是做了2次NAT的, 我觉得FTTH的方案, 也是做了两次NAT的


