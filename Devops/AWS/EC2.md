EC2的集中类型
1. On demand
2. Reserved 有预定时间， 为一年或3年的合同
3. Spot bid出来的
4. Dedicated Hosts 裸金属的机子

EC2 计费
按照小时和分钟计算
1 个 EC2实例运行 24 个小时的价格和 24 个EC2实例运行 1 个小时一样
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
