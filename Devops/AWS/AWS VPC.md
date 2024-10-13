
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
注意: 则个 CIDR 块不能修改. 除非删除了这个 VPC




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
