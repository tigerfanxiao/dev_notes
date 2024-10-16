I would like to find a position as DevOps or SRE site reliability engineer. 
The idea I am interested in is IaC. Infrastructure as Code

# 学习过程
1. 7 月 15 日 完成 AWS System Ops Associate 考试
2. 8 月 24 日 完成 AWS Solution Architect Professional 考试
3. 9 月 25 日 完成 AWS DATA Engineer Associate考试
4. 10 月 1 日 完成Python 基础练习的复习
5. 10 月 3 日 完成 SQL 基础语法的复习
6. 10 月 10 日完成 Pandas 的基础语法复习
7. 
# Job Description
K8s 很重要
AWS production Experience很重要
GCD 很重要, 除了 AWS 之外, 用的也比较多
IaC 很重要
有的需要 React 和 TypeScript 经验
Linux 和 Containerization 很重要
编程语言 Python
需要学习 Cloud Security 相关的知识, 比如 Zero-trust相关的

# Project 项目
## 项目想法

# 西班牙分拣中心Meraki 基础网络改造项目
1. 项目管理 
	1. 专线和 FTTO 主备线路寻源
	2. 施工供应商能力评估
	3. 拉通业务侧和协调施工进度
2. 现场工勘 - 现有网络是 Aruba, 寻找 meraki 方案, 实现欧洲区域自动化管理
3. 无线信道调优 - 与现有仓库网络冲突
4. 方案降本需求, 把吊顶安装修改为对射模式, 和全向天线补位
5. IoT 设备 NTP 无线网络延迟代码优化管理 PC 设置为NTP服务器,  计算 NTP 服务器和工控控制器之间毫秒差修正小车速度. 
6. Meraki 的 API 设计
7. 组网设计, 实现高可用
### 网络自动化项目
1. 使用 K8s 进行部署, 使用 docker, 每个工程师创建一个割接任务时, 都会启动一个 docker. 但是数据库是保存在 volume 中的, mysql. 体现容器化和微服务的思维
2. 数据库设计: 监控设备的 snmp 信息, 比如 cpu 的百分比, 上一次割接的时间. 割接前后的 BGP peer 数量的变化
3. 对设备配置的 mind mind 可视化输出, 方便网络工程师设计割接方案. 
4. 对设备配置, log 非结构化数据, 进行结构化
5. 编排工具怎么设计
6. 采集设备状态信息和配置信息, 进行数据分析. 是不是考虑是 data steam的状态信息分析
7. 项目体现数据库的设计, 选型. NoSQL 还是 SQL
	- 通过数据库的表设计, foreigin key 来减少代码?主要体现 IaC 的思想

# Cloud Computing
### AWS Certificate

Devops Professional 
Network Speciality
Security Speciality Zero Trust
# Questions
1. There are many topics to learn, but I only have 3 months, so I need help from professional or market aspect to identify which topics is more important than others. I would like to make some project to present my skill, in this project I also want to include those important skill stacks. 
2. Maybe I still missing some topic is quite important, so could you help to revise my learning list. 
3. The effort weight is also quite important, as I have limited time. I would like to which topic I need to go more deeper, and which of them I just need to know the basic and concepts. 
# Background
## Education & Certificates
- Bachelor and Master Degree major in Mathematics
- CCIE Cisco Enterprise Internet Expert for Enterprise
- AWS SAA
## Experience
- Python Developer. Automate network cutover for 7K network equipment for Orange Core Network in Huawei 
- Frontend: Basic CSS, JS knowledge. Develop form page with HybirdForm (like a frontend framework) for automating process of asset audit for 2000 Radio site in Germany
- Data Analysis: Make dashboard on low code platform inside of Huawei

# Programming Language
I am going to master 3 programming language. Python, GoLang, JAVA. I have already familiar with Python. JAVA SE. Go haven't begin. 

## Python - weight 30%
Most popular language for SRE 
### Web Framework
- Django
- RestAPI
- One project with Django to implement Restful backend developer
## Go - weight 30%
Very popular language to cloud and distributed system, very high efficient
- Basic 
- Write algorithm question with Go
## Java - weight 40%
Recommended by Borja, more than 90% of the platform is written by JAVA. 
- String Boot

# DevOps Tools
## Docker 
Most popular container technology 

## Kubernetes 
Recommended by Borja, more important than AWS Certification.
Trying to get following certificates. 
- CKA
- CKS
- CKAD
### Grafana 
Very popular tool for monitoring
### Prometheus 
### Elastic Search

## Terraform
A tool to implement IaC

## Ansible
More like a execution tool to maintain servers 
## Linux
- Systemd

### Bind 
DNS for Linux, I think it is quite important as, even in K8S, we need dns for pod 
### SSH
SSH configuration is important
### Bash Script

## Git
## CICD
Two CICD tools I am going to learn
### Gitlab
I can install gitlab by docker
### Jenkins


## Database
### SQL Database
- Postgre
- Mysql - Learned
- SQLite - Learned
### NoSQL Database
- Redis
- InfluxDB 
- MongoDB
## Project Management
### Jira Ecosystem
### Jira
### Confluence
### Agile Methodology

# System Design

Popular system design pattern on the market


# Load Balancer
I am going to learning two LB appliance
### F5
Recommended by Borja. F5 is physical equipment for LB. 
### NGINX
Very popular Software Load Balancer, even can be containerized. 



