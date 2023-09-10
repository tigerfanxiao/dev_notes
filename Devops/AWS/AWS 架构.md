AWS 架构学习 可以 查找 [AWS Architecture BLOG](https://aws.amazon.com/cn/blogs/architecture/)
# 全球服务 vs 非全球服务

全球服务
-   IAM
-   Route53
-   CloudFront
-   SNS
-   SES

Global views but regional
-   S3


# Data Protection

Amazon Macie 保护敏感信息
AWS Key Management Service 保存加密秘钥
AWS CloudHSM 保存基于硬件的的秘钥
AWS Certificate Manager 部署和管理 SSL 和 TLS 认证
AWS Secrets Manager 管理或者循环秘钥. 比如连接数据库的秘钥

### Infrastructure Protection
AWS Shield 这是 Denial of service protection 防止 DDoS 攻击
AWS Web Application Firewall 防止恶意流量
AWS Firewall Manager 控制防火墙策略

### Threat Detection
Amazon GuardDuty 入侵防御
Amazon Inspector 应用安全分析
AWS Config 记录和评估AWS 资源
AWS CloudTrail 追踪用户行为, API使用情况

### Identity Management
AWS IAM 管理登录 AWS 账户和权限
AWS Single Sign-On 云一次登录 SSO 一次登录所有的平台
AWS Cognito 管理等于应用的账户
AWS Directory Service 集成和管理 Microsoft Active Directory
AD connector 允许用Microsoft Active Directory来 登录 AWS
AWS Organizations 管理组织级账号

### Storage
S3 对象存储. 每个文件有一个 key 值, 所有文件在同一个平面上
EFS 文件存储. 按照文件夹结构类似树形结构存储
FSX 面对 windows 的文件存储
EBS 块存储. 适用于高性能计算. 或者虚拟机使用的存储(EC2)
AWS Storage Gateway 用于将本地数据传到 AWS 云
AWS DataSync 也是将本地存储传到AWS 云. 速度是普通的 10 倍
AWS Transfer Family 使用 FTP, SFTP 传输文件给 AWS
### Database
RDS 关系型数据库
Aurora DB 可以 multi-AZ 备份的数据库
Amazon DocumentDB 兼容 MongoDB
DynamoDB 适合存 IoT设备的小数据
ElastiCache 数据库缓存, 相当于 Memcached 可以给 RDS 做缓存
 

### Edge
AWS Snow Family 
AWS Outpost 私有本地的 AWS 云
AWS Wavelength 提供 5G 接入
VMWare Cloud on AWS
AWS Local Zones 运行对延时要求较高的应用

### Computing
EC2
ECS 运行 docker 地方
ECR 存放和管理 docker image 的地方
EKS  用 K8S 管理 docker
Lambda Serviceless


### CDN
CloudFront CDN 技术

### DNS
Route 53

### 负载均衡
ELB 接受用户请求, 将请求发给EC2
ALB


AWS
Fargate
Lambda
SQS
