Generic Routing Encapsulation

GRE tunnel 本质上对 ip 报文的一次封装
如果两个设备经过好几台设备他们是通的. 我们可以在这两台设备之间建立 GRE 隧道. 建立隧道之后, 这两台设备就好像直连的设备一样. 从隧道走一跳就能到对端的设备

GRE 的特点
1. 没有加密, 数据在中间的设备上如果被截取的话, 可以直接解读
2. 可以封装任意的数据, 包括广播, 单播, 组播, apple talk

GRE 对原有的 IP 报头进行封装

IP source  - IP Destination - GRE head - GRE IP Destination - GRE IP Source

GRE配置

```
# R1
int tunnel 1  # 可以认为这是一个新的虚拟接口, 一个新的网段
ip address <本端虚地址> 255.255.255.252 # 这是一个虚地址
tunnel source <本端物理出接口> # 从这个接口出去的流量都进入隧道
tunnel destination <对端物理地址> 255.255.255.252 # 指定隧道对端的 ip 地址

# R4
int tunnel 1 # 这个数字不必要和R1上相同
ip address 192.168.0.2 255.255.255.252
tunnel source gig 0/1 
tunnel destination 192.168.0.1 255.255.255.252

# 这个 tunnel 在 show ip int bri 里可以看到
show int tunnel 1
```

# IPsec
IPsec 技术也是一种隧道技术, 但是它只能封装单播 Unicast. 
IPsec 主要分为两个阶段, 第一个阶段只是是协商policy 中的各种加密, hash, 验证的方法. 
第二阶段做 transform set, 又是算法


1. Provides
	1. confidentiiality: Encryption
	2. Integrity: Hashing
	3. Authentication: PSKs or Digital Signatures
	4. Anti-replay: Applies Serial Numbers to Packets
2. Can encapsulate unicast IP packets
3. Two Modes
	1. Transport Mode: Uses Packet's original header
	2. Tunnel Mode: Encapsulates entire packet
4. Setup Steps
	1. Step #1: Establish an Internet Key Exchange (IKE) Phase 1 tunnel. a.k.a Internet Security Association and Key Management Protocol (ISAKMP) tunnel 为第二阶段商定参数
	2. Step #2 Establish IKE Phase 2 Tunnel


# GRE over IPsec
GRE 可以把任何种类的报文(组播, 广播)封装成 GRE 报文, GRE 报文是单播. 然后我们用 IPsec 对 GRE 进行封装. 所以等于有两层隧道吗

```
#R1
# 首先是第一阶段的配置
crypto isakmp policy 10
encryption aes 
authentication pre-share
group 2 # Diffie-Hellman group 控制秘钥交换过程中 key 的强度

# 如果用了 pre-sharekey, 那么接下去配置
# 如果我们允许所有连接我们的设备, 只要 key mactched 就能通信, 那就是 0.0.0.0 0.0.0.0
# 如果我们只允许某个指定 ip 的设备连接, address要设置成对端设备的 ip 地址
crypto isakmp key xiaokey address 0.0.0.0 0.0.0.0 

# 下面是第二阶段的配置
crypto ipsec transform-set Xiao esp-aes esp-md5-hmac # hash algorithm
mode transport # ipsec 的模式, 不改变原报文头部
exit
# 用 acl 把感兴趣的流量拿出来, 进行加密
ip access-list extended GRE-IN-IPSEC
permit gre any any  # gre协议中的任意流量
exit

# 构建一个 Crypto map把上面的东西汇总起来
crypto map Xiao_VPN 10 ipsec-isakmp
match address GRE-IN-IPSEC
set transform-set Xiao
set peer <对端物理入口地址> 

# 最后把这个 crpyton map应用到R1流量入口. 因为 acl 已经抓取了所有的 GRE 流量
int gig 0/1 # 本端的物理出接口
cryto map Xiao_VPN

# 在R2在重新配置一遍


# 查看 ipsec 第一阶段装填
show crypto isakmp sa
show crypto ipsec sa

```