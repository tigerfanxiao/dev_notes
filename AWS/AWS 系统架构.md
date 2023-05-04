

# Doubts

1.  what is the differnce between Elastic Beanstalk and CloudFormation?
2.  R53 躲在ELB之后， 分配流量
3. 到底什么事高可用 HA High Availability ?


# AWS 服务名字
* storage Gateway Cache you file replicate file to S3
* codeDeploy: deploy your code to EC2 instance
* IoT Greengrass: Connect IoT device to AWS
* AWS Workspace: Desktop as a Service (DaaS) solution
* SES: email messaging platform for businesses and developers.

### APN Consulting Partners

are professional services firms that help customers of all types and sizes design, architect, build, migrate, and manage their workloads and applications on AWS, accelerating their journey to the cloud. APN Consulting Partners often implement Technology Partner solutions in addition to the professional services they offer.

### 5 pillars of a Well Architected Framework

Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization.

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


# 全球服务 vs 非全球服务

全球服务
-   IAM
-   Route53
-   CloudFront
-   SNS
-   SES

Global views but regional
-   S3

# S3


### Multipart uploads

use multithreading to upload large files to S3 buckets in parallel (the parts of the file are uploaded in parallel).

### Cross Region Replication

## S3 Encryption

you can change storage classes and encryption of your objects on the fly.

### Encryption in Transit

SSL/TLS

### Encryption at Rest (Server Side)

-   S3 Managed Keys - SSE-S3
-   AWS key management service, Managed Keys - SSE-KMS
-   Server Side Encryption with Customer Provided Keys SSE-C

### Encryption at client Side

Storage lifecycle

### Restricting Bucket Access

Bucket Policies - Applies across the whole bucket

Object Policies - Applies to individual files

IAMPolicies to Users & Groups - Applies to Users & Groups

### Bucket Policy

Use bucket policy to make entire S3 bucket to public

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow", // allow everything
			"Principal": "*",
			"Action": [
				"S3:GetObject"
			], 
			"Resource": [
				"arn:aws:s3:::BUCK_NAME/*"  // AWS resource name
			]
		}
	]

}
```



# CloudFront

### Origin

This is the origin of all the files that the CDN will distribute. This can be an S3 Bucket, an EC2 instance, an Elastic Load Balancer, or Route53

### Distribution

This is the name given the CDN which consists of a collection of Edge Locations.

### 2 types of Distribution

Web distribution - Typically used for Websites

RTMP - Used for Media Streaming

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2aec5d1a-8697-48fd-a4e3-d71a82d0671c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2aec5d1a-8697-48fd-a4e3-d71a82d0671c/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a94ba2ac-4e60-4280-a2ba-432531e45f13/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a94ba2ac-4e60-4280-a2ba-432531e45f13/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1ef7214d-c558-4905-8f79-3f3717eb6b69/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1ef7214d-c558-4905-8f79-3f3717eb6b69/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/762f353b-e0de-4645-91cd-8a1c97d5b708/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/762f353b-e0de-4645-91cd-8a1c97d5b708/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c42dc0a-91b9-4da3-af63-8533a5463eca/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c42dc0a-91b9-4da3-af63-8533a5463eca/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6072dcdc-eda7-49c2-a6bf-80e191e653c4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6072dcdc-eda7-49c2-a6bf-80e191e653c4/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec6dcd63-453c-4cf1-95df-b304d97d768a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec6dcd63-453c-4cf1-95df-b304d97d768a/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ad0a2e0-bbb7-44c2-acd0-82a6e589ef8f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ad0a2e0-bbb7-44c2-acd0-82a6e589ef8f/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87a0b90f-de80-4a54-8f15-6a3cf71d72a3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87a0b90f-de80-4a54-8f15-6a3cf71d72a3/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cb07dcd-ca59-4ee4-b345-336b37c878ec/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cb07dcd-ca59-4ee4-b345-336b37c878ec/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71144912-5554-4a74-9901-32f23db03e92/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71144912-5554-4a74-9901-32f23db03e92/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42c6d474-331a-4053-89d8-1b4ef8f4eee6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42c6d474-331a-4053-89d8-1b4ef8f4eee6/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b98b7be9-5baa-46d0-9a96-fa5116aac6d7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b98b7be9-5baa-46d0-9a96-fa5116aac6d7/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eea287f7-8e80-4b9f-becd-92446942314d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eea287f7-8e80-4b9f-becd-92446942314d/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/251ced9b-912a-4e61-9342-65c28ecf16b6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/251ced9b-912a-4e61-9342-65c28ecf16b6/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1316cc63-9cbf-49a0-90e5-df3dd6af881b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1316cc63-9cbf-49a0-90e5-df3dd6af881b/Untitled.png)

# Athena

Amazon Athena is interactive query service which enables you to analyse and query data located in S3 using standard SQL

-   Serverless, nothing to provision, pay per query/ per TB scanned
-   No need to set up complex Extract/Transform/Load (ETL) processes
-   Works directly with data stored in S3

Format: "JSON", "Apache Parquet", "Apache ORC" amongst others, but "XML" is not a format that is supported.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0291cfe-b170-48c2-be37-ef46e5a82fe8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0291cfe-b170-48c2-be37-ef46e5a82fe8/Untitled.png)

Macie

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/471d7046-dbf3-453a-9f61-f48bc3067f1d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/471d7046-dbf3-453a-9f61-f48bc3067f1d/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b0de062-4839-4e9d-8cd1-040939760945/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b0de062-4839-4e9d-8cd1-040939760945/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3819b7a8-953c-48fc-8593-fb1a5f5ded3a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3819b7a8-953c-48fc-8593-fb1a5f5ded3a/Untitled.png)

Power User Access allows Access to all AWS services except the management of groups and users within IAM.

# EC2

Amazon Elastic Compute Cloud (Amazon EC2) is just a virtual server (or servers) in the cloud.

Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change.

### Models for EC2

**On Demand**

Allows you to pay a fixed rate by the hour (or by the second) with no commitment

-   users that want the low cost and flexibility of Amazon EC2 without any up-front payment or long-term commitment
-   Applications with short term, spiky, or unpredictable workloads that cannot be interrupted
-   Applications being developed or tested on Amazon EC2 for the first time

**Reserved**

Provides you with a capacity reservation, and offer a significaant discount on the hourly charge for an instance. Contract Terms are 1 Year or 3 Year Teams.But regional

-   Applications with steady state or predictable usage
-   Applications that require reserved capacity
-   users able to make upfront payments to reduce their total computing costs even further

**Spot**

Enables you to bid whatever price you want for instance capacity, providing for even greater savings if your application have flexibale start and end times

-   Applications that have flexible start and end times
-   Applications that are only feasible at very low compute prices
-   Users with urgent computing needs for large amounts of additional capacity
-   If the Spot instance is terminated by Amazon EC2, you will not be charged for a particial hour of usage. However, if you terminate the instance yourself, you will be charged for any hour in which the instance ran.

**Dedicated Hosts**

Physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses.

-   Useful for regulatory requirements that may not support multi-tenant virtualization.
-   Great for licensing which does not support multi-tenancy or cloud deployments.
-   Can be purchased On-Demand (Hourly)
-   Can be purchased as a Reservation for up to 70% off the On-Demand price

### Reserved Pricing Types

-   Standard Reserved Instances
    
    These offer up to 75% off on demand instances. The more you pay up front and the longer the contract, the greater the discount. But you cannot change the region
    
-   Convertible Reserved Instance
    
    These offer up to 54% off on demand capability to change the attributes of the RI as long as the exchange results in the creation of Reserved Instances of equal or greater value.
    
-   Scheduled Reserved Instance
    
    These are available to launch within the time windows you reserve. This option allows you to match your capacity reservation to a predictable recurring schedule that only requires a fraction of a day, a week, or a month.
    

EC2 Instance Type

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/607b95f5-4c73-4ebc-b8fb-0e0e1018297b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/607b95f5-4c73-4ebc-b8fb-0e0e1018297b/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/765cce8d-87cd-41c5-90f2-5564adcc8564/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/765cce8d-87cd-41c5-90f2-5564adcc8564/Untitled.png)

### Spot Instance

To use Spot Instances, you must first decide on your maximum Spot price. The instance will be provisioned so long as the Spot price is Below your maximum Spot price.

-   The hourly Spot price varies depending on capacity and region
-   If the Spot price goes above your maximum, you have two minutes to choose whether to stop or terminate your instance.
-   You may also use a Spot block to stop your Spot Instance from being terminated even if the Spot price goes over your max Spot price. You can set Spot blocks for between one to six hours currently.
-   Spot Instances are useful for the following tasks: Big data and analytics, containerized workloads, CI/CD and testing, web services, Image and media rendering, High-performance computing.
-   Spot Instances are not good for Persistent workloads, critical jobs. databases

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da0d709b-6ba4-4393-bf84-85e028e27eb6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da0d709b-6ba4-4393-bf84-85e028e27eb6/Untitled.png)

### Spot Fleets

A spot Fleet is a collection of Spot Instances and, optionally, On-Demand instances.

The Spot Fleet attemps to launch the number of Spot Instances and On-Demand instance to meet the target capacity you specified in the Spot Fleet request. The request for Spot Instance is fulfilled if there is available capacity and the maximum price you specified in the request exceeds the current spot price. The Spot Fleet also attemps to maintain its target capacity fleet if your Spot instance are interrupted.

-   Set up diferent launch pools. Define things like EC2 instance type, operating system, and Availability Zone
-   You can have multiple pools, and the fleet will choose the best way to implement depending on the strategy you define.
-   Spot fleets will stop launching instances once you reach your price threshold or capacity desire.

### EC2 Placement Groups

There are 3 types of placement groups: Clustered Placement Group, Spread Placement Groups, Partitioned.

Cluster Placement Group:

-   Low Network Latency/High Network Throughput

Spread Placement Group

-   Individual Critical EC2 instances

Partitioned

-   Multiple EC2 instances HDFS, HBase, and Cassandra

### Cluster Placement Groups

A cluster placement group is a grouping of instances within a single Availability Zone. Placement groups are recommended for applications that need low network latency, high network throughput, or both.

### Spread Placement Groups

A spread placement groups is a group of instances that are each placed on distinct underlying hardware.

Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other.

Spread placement groups have a specific limitation that you can only have a maximum of 7 running instances per Availability Zone

### Partitioned Placement Group

When using partition placement groups, Amazon EC2 divides each group into logical segments called partitions. Amazon EC2 ensures that each partition within a placement group has its own set of racks. Each rack has its own network and power source. No two partitions within a placement group share the same racks allowing you to isolate the impact of hardware failure within your application.

### Exam Tips for placement groups

-   A clustered placement group can't span multiple Availability Zones.
-   A spread placement and partitioned group can
-   The name you specify for a placement group must be unique within your AWS account
-   Only certain types of instances can be launched in a placement group (Compute Optimized, GPU, Memory Optimized, Storage Optimized)
-   AWS recommend homogenous isntances within clustered placement groups.
-   You can't merge placement groups
-   You can move an existing instance into a placement group. Before you move the instance, the instance must be in the stopped state. You can move or remove an instance using the AWS CLI or an ASW SDK, you can't do it via the console yet.

### Hypervisor of EC2

AWS originally used a modified version of the Xen Hypervisor to host EC2. In 2017, AWS began rolling out their own Hypervisor called Nitro

### EC2 Hibernat

When you hibernate an EC2 instance, the operating system is told to perform hibernation (suspend-to-disk). Hibernation saves the contents from the instance memory (RAM) to your Amazon EBS root volume. We persist the instance's Amazon EBS root volume and any attached Amazon EBS data volumes.

When you start your instance out of hibernation:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ff7122e-cbb5-41e3-a17f-fb266af9180b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ff7122e-cbb5-41e3-a17f-fb266af9180b/Untitled.png)

With EC2 Hibernate, the instance boots much faster. The operating system does not need to reboot because the in-memory state (RAM) is preserved. This is useful for:

1.  Long-running processes
2.  Service that take time to initialize

EC2 Hibernate preserves the in-memory RAM on persistent storage (EBS)

Much faster to boot up because you do not need to reload the operating system

Instance RAM must be less than 150 GB

Instance families include C3,C4,C5,M3,M4,M5,R3,R4 and R5

Available for Windows, Amazon Linux 2 AMI, and Ubuntu

Instances can't be hibernated for more than 60 days.

Available for On-Demand instances and Reserved Instances.

### EC2 snapshot

To move an EC2 volume from one AZ to another, take a snapshot of it, create an AMI from the snapshot and then use the AMI to launch the EC2 instance in new AZ.

```bash
# Create a snapshot
aws ec2 create-snapshot
```

# EBS

### Exam Tips

-   Termination Protection is turned off by default, you must turn it on. it prevent you fausily delete ebs
-   On an EBS-backed instance, the dafault action is for the root EBS volume to be deleted when the instance is terminated.
-   EBS Root Volumes of your DEFAULT AMI's can be encryped. You can also use a third party tool (such as bit locker etc) to encrypt the root volume, or this can be done when creating AMI's (lab to follow) in the AWS consile or using the API
-   Additional volumes can be encrypted.
-   If we stop the instance, the data is kept on the disk (with EBS) and will remain on the disk until the EC2 isntance is started. If the instance is terminated, then by default the root device volume will also be terminated.

### EBS Snapshots

-   To create a snapshot for Amazon EBS volumes that serve as root devices, you should stop the instance before taking the snapshot.
-   However you can take a snap while the instance is running.
-   You can create AMI's from both Volumes and Snapshots
-   You can change EBS volume sizes on the fly, including changing the size and storage type.
-   Volumes will always be in the same availability zone as the EC2 instance

through the AWS APIs, CLI, and AWS Console, we can perform actions on an existing Amazon EBS Snapshot.

# AMI

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3784b58a-c448-4f4a-bd6d-bf3a70b0bbb8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3784b58a-c448-4f4a-bd6d-bf3a70b0bbb8/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a47606be-7e65-4b5d-873f-1695bcdcb31f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a47606be-7e65-4b5d-873f-1695bcdcb31f/Untitled.png)

All AMIs are categorized as either backed by EBS or backed by instance store. The former means the root device for an instance launched from the AMI is an EBS volume created from an EBS snapshot. The latter means the root device for an instance launched from the AMI is an instance store volume created from a template stored in Amazon S3.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c83f767-3221-4967-b9fc-7c3058d5c51d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c83f767-3221-4967-b9fc-7c3058d5c51d/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04a43cc5-35e4-4581-86af-93e979d8378a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04a43cc5-35e4-4581-86af-93e979d8378a/Untitled.png)

### Encryption at rest

The use of encryption at rest is default requirement for many industry compliance certifications. Using AWS managed keys to provide EBS encryption at rest is a relatively painless and reliable way to protect assets and demonstrate your professionalism in any commercial situation.

### Encrypted EC2 snapshot

-   Create a Snapshot of the unencryped root device volume
-   Create a copy of the Sanpshot and select the encrypt option
-   Create an AMI from the encrypted Snapshot
-   Use that AMI to launch new encrypted instances

### Design Principle

-   Always design for failure. Have one EC2 instance in each availability zone
-   Deploying in Multiple Availability zones will protect against downtime should an Availability Zone be lost.

### command line

```bash
aws s3 mk s3://acloudguru2019-ccp # make a s3 bucket 
# it will fail becasue we haven't grant right to EC2 to connect S3
aws configure
AWS Access Key ID:
AWS Secret Access Key
Default region name: us-east-1
Default output format: # by default, is json
aws s3 mk s3://acloudguru2019-ccp # now we can create the bucket
aws s3 ls # list all the s3 buckets
echo "hello" > hello.txt
aws s3 cp hello.txt s3://acloudguru2019-ccp # upload file to S3

# see configuratio of AWS
cd ~
cd .aws
vim credentials # not secure to say ak/sk on EC2
# to avoid this situation is to use roles

```

# Load Balancer

3 types of Load Balancer

Application Load Balancer (7 level) make intelligent routing decisions

Network Load Balancer (3 level) - Extreme Performance/Static IP address

Classic Load Balancern (fade out) - Test & Dev, Keep Cost Low

Steps

1.  create load balancer
2.  add EC2 instance as target

### Network Load Balancer

is best suited for load balancing of Transmission Control Protocol (TCP), User Datagram Protocol (UDP) and Transport Layer Security (TLS) traffic where extreme performance is required.

### Application Load Balancer

is best suited for load balancing of HTTP and HTTPS traffic and provides advanced request routing targeted at the delivery of modern application architectures, including microservices and containers.

use case:

-   You have a web site with three distinct services each hosted by different web server autoscaling groups.

### Target Group

A target group tells a load balancer where to direct traffic to : EC2 instances, fixed IP addresses; or AWS Lambda functions, amongst others. When creating a load balancer, you create one or more listeners and configure listener rules to direct the traffic to one target group.

# VPC

virtual data center in the cloud

When create a AWS account, we have a default VPC created

### Network ACL

A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups to add an additional layer of security to your VPC.

### Security Group

A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to five security groups to the instance. Security groups act at the instance level, not the subnet level.

Security Group is stateful. It means when you create a inbound rule allowing traffic in, that traffic is automatically allowed back out again. even you delete all the default outbound rule which permits any IP.

We cannot block specific ip address or port using security group, instead use Network Access Control list.

You can specify allow rules, but not deny rules.

### Exam Tips Security Groups

-   All inbound traffic blocked by default
-   All outbound traffic is allowed
-   Changes to Security Groups take effect immediately
-   You can have any numbe rof EC2 instance within a security group
-   You can have multiple security group attached to EC2 instance

### SR-IOV

SR-IOV, or Single Root I/O Virtualization, is a feature of Enhanced Networking used to provide higher networking performance. On a normal EC2 instance, multiple EC2 instances may share a single physical network interface on the EC2 Host. SR-IOV effectively dedicates the interface to a single instance, and bypasses parts of the Hypervisor, allowing for better performance

### VPC Peer

you can peer vpc of other's account

### Network zones

Similar to Availability Zones, network zones are isolated units with their own set of physical infrastructure and service IP addresses from a unique IP subnet. If one IP address from a network zone becomes unavailable, due to network disruptions or IP address blocking by certain client networks, your client applications can retry using the healthy static IP address from the other isolated network zone.

### VPN

A Virtual Private Gateway sits at the edge of your VPC and is a key component when using a VPN. It's responsible for site-to-site connection from on-premises to a VPC.

A customer gateway is a resource that is installed on the customer side and provides a customer gateway inside a VPC.

### Egress internet gateway

Egress internet gateways do not utilize IPv4, they are specifically for use with IPv6 traffic only.

### VPC flow logs

VPC Flow Logs can be created at the VPC, subnet, and network interface levels. VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC.

# Install Apache service

```bash
#!/bin/bash
yum update -y
yum install httpd -y # install apache httpd
service httpd start
chkconfig on # when EC2 reboot, will start httpd server immediately
cd /var/www/html
echo "<html><h1>Hello Cloud Guros Welcome to my Webpage</h1></html>" > index.html
aws s3 mk s3://bucketname
aws s3 cp index.html s3://butcketname  # copy the index.html to s3

# or 
systemctl start httpd # start httpd
systemctl enable httpd # start automatically at each boot time
systemctl status httpd # show the status

```

The EC2 metadata service allows an EC2 instance to access an API running on 169.254.169.254, which returns data about the instance itself. These data include the instance name, the instance image (AMI) ID, and other useful data.

```bash
curl <http://169.254.169.254/latest/user-data>
curl <http://169.254.169.254/latest/meta-data/>

curl <http://169.254.169.254/latest/meta-data/local-ipv4>  # show private ip address
curl <http://169.254.169.254/latest/meta-data/public-ipv4>  # show public ip address

```

### ENI vs ENA vs EFA

**ENI**: Elastic Network Interface - essentially a virtual network card.

**EN**: Enhanced Networking. Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.

**Elastic Fabric Adapter**: A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.

### ENI

An ENI is simply a virtual network card for your EC2 instances. It allows：

-   A primary private IPv4 address from the Ipv4 address range of your VPC
-   One or more secondary private IPv4 addresss from the Ipv4 address range of your VPC
-   One Elastic IP address (IPv4) per private IPv4 address
-   One public IPv4 address
-   One or more IPv4 address
-   One or more security groups
-   a Mac address
-   a source/destination check flag
-   a description

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2bba5dd7-23eb-4ea2-8ad5-01248e8ce8a0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2bba5dd7-23eb-4ea2-8ad5-01248e8ce8a0/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ba97b34-a3c3-43ea-b345-d2adb696ec45/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ba97b34-a3c3-43ea-b345-d2adb696ec45/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eaa08bb1-8916-4927-baa2-6ed7ef7c6ca6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eaa08bb1-8916-4927-baa2-6ed7ef7c6ca6/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0ab7466-07f4-47b3-9e69-6b452b9e120c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0ab7466-07f4-47b3-9e69-6b452b9e120c/Untitled.png)

# AWS Redshift

Redshift is AWS' data warehousing service

# AWS RDS

RDS Reserved instance are available for multi-AZ deployments

In RDS, the maximum value backup retention period is 35 days

When creating an RDS instance, you can select the Availability Zone into which you deploy it.

## Aurora

Amazon Aurora is belong to RDS

Amzon Aurora is 5 times faster than MySQL

By default, RDS has 16TB volume

Two key features

-   Multi-AZ-For Disaster Recovery
-   Read Replicas-For Performance
-   6 copies by default

### OLTP vs OLAP

**OLTP** Online Transaction Processing

-   Manages transaction-oriented applications on the internet in a 3-tier architecture.
-   manages day to day transaction of an organization
-   Used by the end users such as DBA, DB professional.
-   Use simple and standard queries
-   Use Insert, Delete and Update statement
-   Used for Data processing
-   Backups are rarely needed

**OLAP** online Anyalytics Processing

-   Consist software tools that are used for data analysis for business decisions.
-   Used for offline storage of data
-   Used for business analyst, manager, executive for analysis and reporting purposes.
-   Use large and complex queires like aggregation
-   use Select query for fetching the data
-   Allows database information extraction from multiple databases.
-   data is backed up regularly

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42685efd-2e79-49cb-9b94-a185ec3190b0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42685efd-2e79-49cb-9b94-a185ec3190b0/Untitled.png)

Data Warehousing is not impacting you primary database, may impact your secondary database

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ebd7589-8338-448a-9916-ead02d218cad/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ebd7589-8338-448a-9916-ead02d218cad/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d64a0ad-af7d-4d2e-b94e-dd56cc0d88ae/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d64a0ad-af7d-4d2e-b94e-dd56cc0d88ae/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efadb40f-3f33-49ff-b334-a0e9055ed852/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efadb40f-3f33-49ff-b334-a0e9055ed852/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c16ad46e-a3d0-4e65-b76c-348544c2ec1a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c16ad46e-a3d0-4e65-b76c-348544c2ec1a/Untitled.png)

lab of install RDS

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16c1d170-e957-478a-9f00-99e4da15de9a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16c1d170-e957-478a-9f00-99e4da15de9a/Untitled.png)

criteria affecting your billing for RDS

-   Clock hours of server time
-   additional storage
-   number of requests

### Alias & CNAME

Alias Records have special functions that are not present in other DNS servers. Their main function is to provide special functionality and integration into AWS services. Unlike CNAME records, they can also be used at the Zone Apex, where CNAME records cannot. Alias Records can also point to AWS Resources that are hosted in other accounts by manually entering the ARN

# AWS Marketplace

AWS Marketplace is a digital catalog with thousands of software listings from independent software vendors that make it easy to find, test, buy, and deploy software that runs on AWS

# CloudFormation

AWS CloudFormation is a service that helps you model and set up your Amazon Web Servies resources so that you can spend less time managing thoses resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resoueces that you want (like Amazon EC2 instances or Amazon RDS DB instances), and AWS CloudFormation takes care or privisioning and configuring thoese resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; AWS CloudFormation handles all of that

### ESlastic Beanstalk & CloudFormation

Elastic Beastalk and CloudFormation are both Fee services, however the resouces they provision (such as EC2 instances) are not free

-   Elastic Beanstalk is limited in what it can provision and is not programmable.
-   CloudFormation can provision almost any AWS service and is completely programmable.

# Billing

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7430f626-de9e-4f48-94a4-1e1e6d13eaf1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7430f626-de9e-4f48-94a4-1e1e6d13eaf1/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee7dcc94-e8bd-4846-918b-55270382961e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee7dcc94-e8bd-4846-918b-55270382961e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2842fc1b-e0a8-4e30-9c8a-9a76434268b3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2842fc1b-e0a8-4e30-9c8a-9a76434268b3/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/567f5896-a72c-476b-aeee-bd429c7990b4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/567f5896-a72c-476b-aeee-bd429c7990b4/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dddd2bd-7cfa-44dc-a162-0c0f7cf15541/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dddd2bd-7cfa-44dc-a162-0c0f7cf15541/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87e66a88-d1f8-4797-a2c4-6212aec48627/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87e66a88-d1f8-4797-a2c4-6212aec48627/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee404be9-aa3b-4c1b-9384-701bf2788caa/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee404be9-aa3b-4c1b-9384-701bf2788caa/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef7846ab-714c-4801-b693-501de3f7aa4f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef7846ab-714c-4801-b693-501de3f7aa4f/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f105d6a0-f037-4f11-aeb9-5d88289d82ef/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f105d6a0-f037-4f11-aeb9-5d88289d82ef/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d96293b-c783-457b-92cb-25025ed96df1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d96293b-c783-457b-92cb-25025ed96df1/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c167386b-2908-4ce3-9278-883a8a8ab231/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c167386b-2908-4ce3-9278-883a8a8ab231/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f94efc72-c870-410a-bc4d-9fa5473bb84c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f94efc72-c870-410a-bc4d-9fa5473bb84c/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47ba912f-3763-4602-9a14-8f02b5ddee34/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47ba912f-3763-4602-9a14-8f02b5ddee34/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d58472d-eba8-4e30-a919-30bb98284c75/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d58472d-eba8-4e30-a919-30bb98284c75/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fe32ae80-4265-4af7-a0a8-dd3b01b350b6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fe32ae80-4265-4af7-a0a8-dd3b01b350b6/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1c0c988-ba5d-40e8-8cfa-d1710bc58990/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1c0c988-ba5d-40e8-8cfa-d1710bc58990/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7be6ad41-236c-4b6d-902b-52211cd4e5c8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7be6ad41-236c-4b6d-902b-52211cd4e5c8/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7fd63d3b-471e-4346-a26f-34b2405c48cc/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7fd63d3b-471e-4346-a26f-34b2405c48cc/Untitled.png)

# AWS Cost Explorer

AWS Cost Explorer is a free tool that you can use to view your costs and usage. You can view data up to the last 13 months, forecast how much you are likely to spend for the next three months, and get recommendations for what Reserved Instances to purchase. You can use AWS Cost Explorer to see patterns in how much you spend on AWS resources over time, identify areas that need further inquiry, and see trends that you can use to understand your costs. You can also specify time ranges for the data, and view time data by day or by month

# Amazon EMR

Amazon EMR is a web service that makes it easy to process large amounts of data efficiently.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d41eca18-9d5a-47fd-bf66-b302738cbf58/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d41eca18-9d5a-47fd-bf66-b302738cbf58/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/686a9b76-0e7e-465d-9313-e345d9fa444e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/686a9b76-0e7e-465d-9313-e345d9fa444e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e420f0e1-4ff8-4013-8dc0-138ddd0317db/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e420f0e1-4ff8-4013-8dc0-138ddd0317db/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a49d43c-3bf1-47cc-a8e1-876aaac5bbbd/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a49d43c-3bf1-47cc-a8e1-876aaac5bbbd/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/860c2a05-f18c-49fa-bf79-26b2fc5cb28d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/860c2a05-f18c-49fa-bf79-26b2fc5cb28d/Untitled.png)

# Resource Group & Tagging

A tag is a label that you or AWS assigns to an AWS resource. Each tag consists of a key and a value. For each resource, each tag key must be unique, and each tag key can have only one value. You can use tags to organize your resources, and cost allocation tags to track your AWS costs on a detailed level. After you activate _cost allocation tags_, AWS uses the cost allocation tags to organize your resource costs on your cost allocation report to make it easier for you to categorize and track your AWS costs.

AWS provides two types of cost allocation tags, an AWS generated tags and user-defined tags. AWS defines, creates, and applies the AWS generated tags for you, and you define, create, and apply user-defined tags. You must activate both types of tags separately before they can appear in Cost Explorer or on a cost allocation report.

# Resource Groups

You can use resource groups to organize your AWS resources. Resource groups make it easier to manage and automate tasks on large numbers of resources at one time. This guide shows you how to create and manage resource groups in AWS Resource Groups.

Resource Groups in combination with AWS Systems manager allow you to control and execute automation against entire fleets of EC2 instanes, all at the push of a button.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/982e7ff3-54c5-4e73-9a9c-a0888f19ca74/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/982e7ff3-54c5-4e73-9a9c-a0888f19ca74/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e6dbaa1-f547-4f9b-bf71-01e2ceefaaf7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e6dbaa1-f547-4f9b-bf71-01e2ceefaaf7/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17435d8e-9cbc-4a30-a33b-2a3c03ac86dd/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17435d8e-9cbc-4a30-a33b-2a3c03ac86dd/Untitled.png)

# Tag Editor

is a global service that allows us to discover resource and to add additional tags to them as well. Newer regions may takes some time to be compatible with tag editor

# AWS Organization

AWS Organizations is an account management service that eables you to consolidate multiple AWS accounts into an organization that you create the centrally manage.

Available in two feature sets:

-   Consolidated billing
-   All features

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/11650c4c-7ff5-4cb1-968a-f30761eb0ca7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/11650c4c-7ff5-4cb1-968a-f30761eb0ca7/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b5c41937-e749-4d01-ad66-48c04cb1bbd0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b5c41937-e749-4d01-ad66-48c04cb1bbd0/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6385e84d-fc75-4eff-b0cf-bf202200f12b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6385e84d-fc75-4eff-b0cf-bf202200f12b/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e40637ab-c9a9-4f03-ae28-f3b82b4d5fea/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e40637ab-c9a9-4f03-ae28-f3b82b4d5fea/Untitled.png)

SCP service control policy

Tag policies

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49acfee6-681d-45fe-86d7-43f483877e48/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49acfee6-681d-45fe-86d7-43f483877e48/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb4718dd-28a7-451c-8d69-08fc14c4c67b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb4718dd-28a7-451c-8d69-08fc14c4c67b/Untitled.png)

# CloudTrail

The difference between CloudWatch and ClouldTrail

-   CloudWatch monitors performance
-   ClouldTrail monitors API calls in the AWS platform

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09ee3f96-70aa-4246-9ba3-66dba16cc15f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09ee3f96-70aa-4246-9ba3-66dba16cc15f/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2e4a7113-d044-45f8-ae7b-2993f5323ec2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2e4a7113-d044-45f8-ae7b-2993f5323ec2/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0013001b-c1af-4ea0-8d9e-96f2a9d621a4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0013001b-c1af-4ea0-8d9e-96f2a9d621a4/Untitled.png)

# AWS Quick Starts

AWS Quick Start is a way of deploying environments quickly, using CloudFormation templates built by AWS Solutions Architects who are experts in that particular technology.

# AWS Landing Zone

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9f6537e-fc43-4896-bfd0-b10c9bf79380/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9f6537e-fc43-4896-bfd0-b10c9bf79380/Untitled.png)

# AWS Calculator

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c565a1e0-12e1-4512-a39c-e39adf78e8c4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c565a1e0-12e1-4512-a39c-e39adf78e8c4/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddb3d3e5-cf78-4239-b2e2-e779b55b71e1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddb3d3e5-cf78-4239-b2e2-e779b55b71e1/Untitled.png)

# AWS Compliance

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e554cc3-20b3-4348-bbb1-120e86bcbce5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e554cc3-20b3-4348-bbb1-120e86bcbce5/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a3b2688-bcf0-47b7-ab14-8c86e0c45d36/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a3b2688-bcf0-47b7-ab14-8c86e0c45d36/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb6c129-909d-4303-934c-f9f3243ac713/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb6c129-909d-4303-934c-f9f3243ac713/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32f0213f-c1f4-4276-8430-4dc22eacf10d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32f0213f-c1f4-4276-8430-4dc22eacf10d/Untitled.png)

# AWS Shield

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/587d42a2-8adc-492c-ae2d-92e777fd2a20/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/587d42a2-8adc-492c-ae2d-92e777fd2a20/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b6d5307-278e-476b-8517-2a9f156f0cc2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b6d5307-278e-476b-8517-2a9f156f0cc2/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c05e375-45b0-49df-abba-b1635d384115/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c05e375-45b0-49df-abba-b1635d384115/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/19606153-65ec-4e71-89eb-a431cf7dd926/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/19606153-65ec-4e71-89eb-a431cf7dd926/Untitled.png)

AWS Inspector, is used for inspecting EC2 instrance for vulnerabilities.

AWS Trusted advisor inspects your AWS account as a whole (not just EC2). Check:

-   Security
-   Cost Optimization
-   Performance
-   Falut Tolerance

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e626d6a-3b98-4c0a-831b-601e7a5c2e4f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e626d6a-3b98-4c0a-831b-601e7a5c2e4f/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5bfd4d4-196e-4700-aaf8-deecaccf77af/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5bfd4d4-196e-4700-aaf8-deecaccf77af/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13406b8e-8bb4-4369-9f1e-cb08878e5a04/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13406b8e-8bb4-4369-9f1e-cb08878e5a04/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b15538c9-d509-46da-a1cc-cb14996282c5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b15538c9-d509-46da-a1cc-cb14996282c5/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca233357-71ad-42ad-a459-cf991bc71e34/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca233357-71ad-42ad-a459-cf991bc71e34/Untitled.png)

A HIPAA certification attests to the fact that the AWS Platform has met the standard required for the secure storage of medical records in the US

A PCI DSS Level 1 certification attests to the security of the AWS platform regarding credit card transactions.

# AWS CodeCommit

is a fully-managed source control service that hosts secure Git-based repositories. It makes it easy for teams to collaborate on code in a secure and highly scalable ecosystem. CodeCommit eliminates the need to operate your own source control system or worry about scaling its infrastructure. You can use CodeCommit to securely store anything from source code to binaries, and it works seamlessly with your existing Git tools. This is not what you are investigating.

# Elasticache

You can use Elasticache to store the results of often-used queries, and this will allow quicker retrieval of this data

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfedb717-a627-428a-bc09-999ee951f5a6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfedb717-a627-428a-bc09-999ee951f5a6/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c098e6ea-bef2-47f2-b931-a1588a7d18a7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c098e6ea-bef2-47f2-b931-a1588a7d18a7/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3db624d3-df7f-49e5-8ee7-e97342a0543b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3db624d3-df7f-49e5-8ee7-e97342a0543b/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ea37fc2-0ca4-4d92-8c59-01017ad29816/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ea37fc2-0ca4-4d92-8c59-01017ad29816/Untitled.png)

# AWS Command Line

# HPC

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6ec08e1-de45-4b7d-bbfd-43d21a1f5f06/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6ec08e1-de45-4b7d-bbfd-43d21a1f5f06/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18379be8-ec1a-42a0-81de-44bf746ac27a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18379be8-ec1a-42a0-81de-44bf746ac27a/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9abcf259-38da-4d71-a6aa-7ccb806bb852/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9abcf259-38da-4d71-a6aa-7ccb806bb852/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a44f5c29-62a6-4bde-9c54-f0a5a2ae182e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a44f5c29-62a6-4bde-9c54-f0a5a2ae182e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86d3c40b-bdb3-48ab-90d2-920057989b07/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86d3c40b-bdb3-48ab-90d2-920057989b07/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a59d7387-6b90-485a-9365-4d402ea333aa/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a59d7387-6b90-485a-9365-4d402ea333aa/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0aab4e1-248e-408c-9dcf-2edbd2f22cc0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0aab4e1-248e-408c-9dcf-2edbd2f22cc0/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/797ed39d-fa0b-443a-9cb8-bba8845ed1f3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/797ed39d-fa0b-443a-9cb8-bba8845ed1f3/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1675c28-323b-48cb-b707-4073b57aa2ef/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1675c28-323b-48cb-b707-4073b57aa2ef/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f6ebe40d-f3aa-46c0-a413-394c4f63e301/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f6ebe40d-f3aa-46c0-a413-394c4f63e301/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87c0e004-7745-4aa1-81d2-0f832a7c6a4d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87c0e004-7745-4aa1-81d2-0f832a7c6a4d/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0bbec53e-f893-48ed-a073-4605bfdeddae/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0bbec53e-f893-48ed-a073-4605bfdeddae/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f805dda0-5d65-498b-a6f9-ebc1903dd943/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f805dda0-5d65-498b-a6f9-ebc1903dd943/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcfb1584-fa95-4eb3-92b9-2426d056cc8d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcfb1584-fa95-4eb3-92b9-2426d056cc8d/Untitled.png)

# SWF

domain: A collection of related workflows

Amazon SWF ensures that a task is assigned only once and is never duplicated.

# Lambda

'Serverless' computing is not about eliminating servers, but shifting most of the responsibility for infrastructure and operation of the infrastructure to a vendor so that you can focus more on the business services, not how to manage the infrastructure that they run on. Billing does tend to be based on simple units, but the choice of services, intended usage pattern (RIs), and amount of capacity needed also influences the pricing.

ALB, Cognito, Lex, Alexa, API Gateway, CloudFront, and Kinesis Data Firehose are all valid direct (synchronous) triggers for Lambda functions. S3 is one of the valid asynchronous triggers.

Lambda billing is based on both The MB of RAM reserved and the execution duration in 100ms units.