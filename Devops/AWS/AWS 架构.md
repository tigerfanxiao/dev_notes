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
AWS Key Management Service 保存加密秘钥. 数据在传输过程中加密
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
Aurora DB 支持 Mysql 和 Postgre 可以 multi-AZ 备份的数据库
Amazon Redshift 数据湖
Amazon DocumentDB 兼容 MongoDB
DynamoDB 适合存 IoT设备的小数据
ElastiCache 数据库缓存, 相当于 Memcached/Redis 可以给 RDS 做缓存
 
### Network
VPC
AWS Transit Gateway 可以将多个本地和多个 vpc 进行互联
AWS PrivateLink 本地专线连接 VPC
AWS Direct Connect Gateway 本地专线连接 AWS, 不经过 internet
Route53 DNS
AWS Global Accelerator 利用AWS 全球网络来加速
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
AWS CloudFront CDN 技术

### 负载均衡
ELB 接受用户请求, 将请求发给EC2
ALB 后面可以跟 Lambda


### Account Management Service
AWS Control Tower 创建和管理多账户环境
AWS Organization 集中管理多个 AWS 账户
AWS Budget 管理开销

### Provision Service
AWS CloudFormation 自动化部署 AWS 资源, Infrastructure as Code
AWS Service Catalog 管理你的 AWS 产品
AWS OpsWork 自动化部署工具,类似 puppet 和 chef
AWS Marketplace 购买第三方软件的地方

### Operation Service
Amazon CloudWatch 监控你的服务和日志
AWS Config 评估你对 AWS 资源的配置
AWS CloudTrail 记录所有用户的行为
AWS System Manager 可以用来给 EC2 批量打补丁
Amazon X-ray 帮助我们 debug 自己搭建的应用

### Machine Learning
Amazon Kendra 智能查询
Amazon Personalize 基于个人特性的推荐系统
Amazon Lookout for metrics 数据分析, 发现数据的突然变化, 并找出原因
Amazon forecast 建立预测模型
Amazon Fraud Detector 发现潜在的欺诈
Amazon Rekogition 从图片和视频中抓取信息
Amazon Polly 将文本转化为语音
Amazon Transcribe 将语音转化为文本
Amazon Lex 聊天机器人 chat bot
AWS DeepRacer 自动驾驶小车
CodeGuru 代码审查工具

AWS
Fargate
SQS
