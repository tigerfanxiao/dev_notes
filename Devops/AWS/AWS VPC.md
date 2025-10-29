### VPC 
- VPC 是不能跨区域的
- 一个账户想可以创建多个 VPC
- 一个 VPC 可以扩多个 AZ

### 创建 VPC
一个 VPC, 这个 CIDR 的网段有三种选择
10.0.0.0/8
172.16.0.0/12 从 172.16.0.0 到172.31.255.255
192.168.0.0/16 从 192.168.0.0 到 192.168.255.255

一般是选 /16-/28 的子网掩码, 比如 10.0.0.0/16
在这儿大 CIDR 块里面, 可以创建很多小的子网 subnet
注意: 
- 一旦创建 CIDR 块不能修改. 除非删除了这个 VPC
- 可以创建第二个 CIDR

云原生思维: 
- 服务器不在是静态的 IP 地址, 而是让 VPC 自动分配给EC2 IP 地址. 我们只是使用EC2私有的域名
- 在路由表中指定网关的时候, 也是可以指定一个资源

### Subnet
- 每个子网中的  0-3, 255的地址是预留的. 比如 172.16.100.0 - 172.16.100.3, 172.16.100. 但是本质子网内部都用 dhcp, 所以 IP 地址其实不用太关心
- 对于公有子网, 需要开启自动分配公网 ip 地址, 指定 Internet Gateway 才能访问公网

### Elastic IP
每一次 EC2重启, 被分配的公网地址都会变. 为了解决这个问题, 就有了 Elastic IP. 用户可以独享这个 IP 地址, 关联到指定 EC2的ENI, 直到释放
- 做实验一定别忘记释放这个 EIP, 否则要收费

### Elastic Network Interface
EC2的网卡
- Primary Network Interface 是默认的, 无法删除, 子网和 IP 地址也无法修改
- 可以增加和删除其他网卡

### Internet Gateway
- Internet Gateway 是提供的一对一的(NAT)双向服务的, 即可以访问公网, 也可以让公网访问. 注意这里是假设没有 NAT Gateway 的情况下, EC2所在的子网直接指了默认路由到 Internet Gateway. 此时有多少个 EC2, Internet Gateway上就有多少个公网地址
- 默认VPC 是自己会连接到 Internet Gateway, 且已经配置完成默认路由
- 自己创建的 VPC 需要手动连接的 Internet Gateway, 且需要手动配置默认路由
- 在配置了NAT Gateway 之后, NAT Gateway 相当于 PAT, 一个公网地址可以被多个私网地址使用. 在 EC2配置了 Auto-Scaling 的情况下, 因为 EC2数量是动态的, 往往需要使用 NAT Gateway 来节约公网 IP 地址资源. 但是此时 EC2可以访问公网, 但是公网不能访问 EC2, 这是单向的. 同时我们用ELB 来接受物联网的请求, 此时互联网的请求不经过 NAT Gateway, 直接到达 ELB 后, 被分发到 EC2 上
- Egress Only Internet Gateway 用于 IPv6, 保证流量只能出, 不能进
### Security Group
- 安全组是属于某一个 VPC 的, 但是是关联到 EC2的网卡 ENI 上的
- 默认情况下限制所有进入的流量, 放行所有出去的流量. 
- 一个安全组可以关联到多个实例, 一个实例也可以关联多个安全组
- 安全组的源也可以是一个安全组

### Network ACL 
- 每一个 VPC 都会有一个访问控制列表. 默认是放行所有
- 用来控制离开和进入子网的流量
- 无状态的, 必须进入和出去都要控制
- 一般是使用安全组, 不用 Network ACL
### VPC Flow Logs
VPC Flow Logs 可以发到 CloudWatch 或者 S3 上面
- VPC Flow logs are 
- not real time
- VPC Flow Logs do not capture all traffic
	- traffic to Amazon DNS
	- traffic to windows license activation
	- traffic to 169.254.169.254
	- traffic to DHCP
	- traffic for VPC router
- VPC 


