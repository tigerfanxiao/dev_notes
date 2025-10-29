每一个Region的镜像不一样. 在 us-east-1的镜像最丰富, 最便宜
每个Region的镜像ID也不一样. 所以在使用Cloudformation中提供镜像ID, 每个区域都不一样


Amazon Linux 2的话一般是用 4.14 Kernel版本
一般做实验选T2.micro. 但是CPU不能百分百运行(平均). 使用积分制运行
T2/T3可以选择 unlimited 模式, 但是要收钱
收费计算器
amazon ec2 pricing on-demand 

M5/M4
如果用SSD是临时的, 关机和开机后, 可能数据丢失. 只有EBS才是持久的


### Marketplace
Marketplace 里面都是非AWS的其他厂商提供的官方镜像
如果看到镜像上 BLY 表示AWS只是提供镜像, 但是没有授权. 如果是看到Standard然后又按照小时收费的. 有Cisco, PaloAlto, Fortinet

Region
每个Region的价格都不一样, 韩国的Region bug比较多

AZ
AZ之间可能相差20-30公里, 完全独立的供电, 完全独立的制冷

租户mode
1. Share Tenancy
2. Dedicated Instance 物理机里只有你的实例, 但是物理机在哪个不确定
3. Dedicated Host 指定了物理机, 可以获得物理信息. 一般是面向带着授权的应用

EC2的集中类型
1. On demand
2. Reserved 有预定时间， 为一年或3年的合同
	1. 区分 all upfront 全部支付
	2. Partial upfront 支付部分
	3. No upfront 不支付
3. Spot Market  
4. Compute Saving Plans 承诺最低消费. 可以有优惠
	1. 计算Saving Plans 最高可以节省 66%
	2. EC2实例 最高到72%
5. Dedicated Hosts 裸金属的机子

EC2 计费
按秒计费(仅Amazon Linux和Ubuntu)
按小时计费(其他所有操作系统)

按照小时
1 个 EC2实例运行 24 个小时的价格和 24 个EC2实例运行 1 个小时一样

Placement Group 
让所有的虚拟机在一个AZ, 尽量在一起
1. Cluster
2. Partition
3. Spread

Capacity Rerservation 预留资源 (只要解决实例起不来的情况), 选择open的情况
因为遇到大客户, 某一种cpu对应的设备的实例起不来. 这种时候, 可以设定时间预留部分实例
预留的话, 第一个小时, 就收钱

IAM角色
权限设置好, 不用密码就能访问资源

User Data
可以理解为启动机器时, 刷的脚本
```shell
#!/bin/bash
yum install -y python3
```

# ENI
primary ENI 是不能删除, 不能动的. 在低端的 EC2中, 网卡数量有限制
### Metadata
metadata token reponse hop limit 令牌只有一跳
```shell
# 这个ip地址是固定的
curl http://169.254.169.254/latest/meta-data/
curl http://169.254.169.254/latest/meta-data/local-ipv4
```
测试负载均衡的时候, 可以用来api查看实例所在的region, 实例编号等元数据信息


# MAC terminal 访问 ECS

```bash
# 1. downlaod .pem file
# 2. open terminal with the directory where you save the .pem file
# 3. change the privilege of .pem file
chmod 400 mykey.pem
# 4. connect to EC2 
ssh ec2-user@34.229.236.39 -i mykey.pem
# configure and update EC2 instance
sudo su
yum update 
```

# Auto Scaling Group
Auto Scaling Group的实例可以跨多个AZ
Auto Scaling Group 有template

