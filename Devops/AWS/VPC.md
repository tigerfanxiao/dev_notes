VPC 是自己私有网络. 里面放着我们所有的服务. 
在创建 vpc时需要指定一个网段. 默认情况下, VPC 中的所有组件都是互通的. 如果这些组件比如EC2需要隔离, 就需要用Security Group. 在 Security Group 中可以限定某些端口可以被访问

一个VPC中可以设置多个子网, 这些子网默认是打通的吗?

如果 VPC 内的组件想要访问 Internet, 就需要用 NAT Gateway. 
如果你想让外界访问你 VPC 内部的 EC2, 那就要用 Internet Gateway
如果你想控制什么流量能够进入 VPC, 什么流量能够出去, 则使用 network access control list

NACL
用来控制访问 VPC 的流量

Security Group
用来控制访问EC2实例的流量

NACL和Security Group的区别
**NACLs operate at the subnet level and control traffic in and out of a VPC, while Security Groups operate at the instance level and control traffic to and from individual EC2 instances**.



### VPC Endpoint
AWS上有不少共有服务, 不在 VPC内, 比如 S3. 如果VPC 内部的组件要访问S3, 需要通过 S3 VPC Endpoint
ELB VPC Endpoint 允许负载均衡器连接 VPC 内部组件

### SR-IOV


SR-IOV, or Single Root I/O Virtualization, is a feature of Enhanced Networking used to provide higher networking performance. On a normal EC2 instance, multiple EC2 instances may share a single physical network interface on the EC2 Host. SR-IOV effectively dedicates the interface to a single instance, and bypasses parts of the Hypervisor, allowing for better performance



### Egress internet gateway
用于IPv6出口
### VPC flow logs


### ENI vs ENA vs EFA

**ENI**: Elastic Network Interface 本质上是虚拟的网卡, 可以浮动的. 比如我有EC2 的集群, 一个 EC2 挂了, 新拉起的 EC2可以使用同一个 ENI 网卡

**EN**: Enhanced Networking. Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.

**Elastic Fabric Adapter**: A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.