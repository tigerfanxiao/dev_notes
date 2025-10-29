AWS 架构学习 可以 查找 [AWS Architecture BLOG](https://aws.amazon.com/cn/blogs/architecture/)

# Concepts

CSP Cloud Service Provider 
DR Disaster Recovery 
RTO Recovery Time Objective 灾难发生后, 多少时间可以恢复服务
RPO Recovery Point Objective 灾难发生前, 多少时长的数据可以被丢失
SAML Security Assertion Markup Language 允许用同一个账户密码访问多个网站

# 产品
Grafana 使用Data Visualizaion 工具. AWS提供了 Grafana workspace 
Splunk 大数据分析平台
### 传统时间和AWS世界

- 在传统世界做Devops不是不可能，而是非常困难。 基本上只能做到局部或者一个很小范围的自动化。 因为:
	- 设备的品牌不同，配置命令不同
	- 不同设备的通信协议不同
	- 不同设备的日志格式不同
	- 不同设备的账户管理复杂 
在整个大网做自动化是很困难的。 

#### 国内和国外
国内采买IaaS的更多, 基本是把传统架构照搬到云上
国外采买PaaS的更多, 使用云原生架构 Serverless理念应用 
### 云原生
* 有的服务一开始就在云上搭建。 架构和传统世界不同。 比如Lambda这种Serverless就是云原生的概念。 
## AWS的优势

- API是统一的, 使用AWS Boto3 SDK. 本质上所有的操作都是API, 图形界面只是副产品
- 整合度高. S3可以存储任何东西, 所有的API行为都在cloudtrail上有日志, Cloudwatch保存了所有资源的指标, AWS config控制所有资源
- 账户管理. 统一权限和安全控制
- 人力成本极大降低
- 系统级的可复制到Devops方案. 只有安全还需要专门的团队. 网络和存储不需要专门的团队
### 成本
成本可以分为 CAPEX和OPEX
- CAPEX指硬件投资成本. 比如机房租借和维护. 设备购买和折旧
- OPEX指运营成本
#### 传统世界的成本
- 机房维护
- 网络维护
- 安全维护
- 系统维护
- 数据库维护

#### 云的成本
- 如果把传统架构直接搬到云上, 使用云的成本是高于传统架构的. 当对传统架构进行云原生改造后, 云的成本的才会和传统架构持平. 所以并不是所有的企业业务都适合上云
- 云更适合创业型企业. 也就是业务快速增长, 或者业务只在短时间有爆发, 或者开工3个月, 开工吃三年的企业. 云原生对那些业务稳定的企业并不具备太多优势.

# 全球服务 vs 非全球服务

全球服务
-   IAM
-   Route53
-   CloudFront
-   SNS
-   SES

Global views but regional
-   S3
-  DynamoDB


# AWS 服务


### 情景分类

### API

### Webhook
使用Amazon API Gateway
### On-premise
- 对on-premise设备上的指标metrics进行监控. 用 AWS Application Discovery Agent
- 对On-premise的迁移方案进行分析 AWS Migration Hub

### Data Protection

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


# 架构分析方法

### 5 Pillars of Design

- Cost Optimization 成本优化
	- 分析支出和确定支出归属
	- 使用经济高效的资源
- Reliability 可靠性
	- 从故障恢复, 测试恢复的过程
- Performance efficiency 性能效率
	- 减少延迟, 使用无服务架构
	- 纳入监控, 资源的饱和度
- Operational Excellence 卓越运营
	- 利用代码执行运营, 自动化
- 可持续性
	- 最大限度的提高资源利用率
- Security 安全性

