目前已经监控了 70 + AWS 服务

监控工具, 监控范围
1. 基础架构(虚拟或物理的)
2. 应用本身
特性
*  只有两种监控频率 5 分钟(默认), 1 分钟(详细)
*  可以触发告警消息sns. Billing Alarm
-  区别于CloudTrail, CloudTrail是做审计的

监控的范围
Compute: EC2 Instance, Autoscaling Groups, Elastic Load Balancer, Route52 Health Checks
Storage & Content Deliver, EBS Volumes, Storage Gateways, CloudFront
physical server: CPU, Network, Disk, Status Check

### 最佳实践
1. 监控 EC2 被 ssh 的人. 将 EC2 上 SSH 的 log 发给 CloudWatch, 然后在 CloudWatch 上做过滤, 查找某个人连续 5 次 ssh 登录失败, 并发送 Alarm 告警