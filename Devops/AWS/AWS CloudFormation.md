Provision AWS Resource
实现了 Infrastructure as Code 通过模板来构建基础设施. 而所谓的模板是文本文件
- 用 yaml 或者 json 来管理配置
- 用 git 来记录配置变动的版本
- 支持模板嵌套

功能
1. 快速构建测试床, 在测试完成后, 又可以马上删除, 来节约资源
2. 快速在其他 region 构建备份站点


区分 Template和Stack

### Stack
- Parameters  在交互界面可以选择
- Mappings 要怎么用???


下面是创建 stack 的例子
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: "Deploys basic networking and an EC2 instance"
Parameters: # 在页面可以选择
	InstanceTypeParameter:
		Type: String
		Default: t3.micro
		AllowedValues:
			- t3.micro
			- t3.small
			- t3.medium
			- t3.large
		Description: "Select an Instance Type"
Mappings:
	Subnet1Config:
		VPC1:
			CIDR: 10.0.0.0/16
		Public1:
			CIDR: 10.0.0.0/24
		Public2:
			CIDR: 10.0.1.0/24
Resources:
	VPC1:
		Type: "AWS::EC2::VPC"
		Properties:
			EnableDnsSupport: "true"
			EnableDnsHostnames: "true"
			CidrBlock: !FindInMap
				- Subnet1Config
				- VPC1
				- CIDR
			Tags:
				- Key: Name
				  Value: ACG_VPC
	VPC1PublicSubnet1:
		Type: "AWS::EC2::Subnet"
		Properties:
			VpcId: !Ref VPC1
			AvailabilityZone: !Select
				- "0"
				- !GetAZs us-east-1
			MapPublicIpOnLaunch: "true"
			CidrBlock: !FindInMap
				- Subnet1Config
				- Public1
				- CIDR
			Tags:
				- Key: Name
				  Value: ACG_PUBLIC_1
	VPC1PublicSubnet2:
		Type: "AWS::EC2::Subnet"
		Properties:
			VpcId: !Ref VPC1
			AvailabilityZone: !Select
				- "1"
				- !GetAZs us-east-1
			MapPublicIpOnLaunch: "true"
			CidrBlock: !FindInMap
				- Subnet1Config
				- Public2
				- CIDR
			Tags:
				- Key: Name
				  Value: ACG_PUBLIC_2
	InternetGateway1:
		Type: "AWS::EC2::InternetGateway"
		Properties:
			Tags:
				- Key: Name
				  Value: ACG_IGW
	GatewayToInternet1:
		Type: "AWS::EC2::VPCGatewayAttachment"
		Properties:
			VpcId: !Ref VPC1
			InternetGatewayId: !Ref InternetGateway1

	PublicRouteTable1:
		Type: "AWS::EC2::RouteTable"
		Properties:
			VpcId: !Ref VPC1
			Tags:
				- Key: Name
				  Value: ACG_RT
	PublicRoute1:
		Type: "AWS::EC2::Route"
		DependsOn: GatewayToInternet1
		Properties:
			RouteTableId: !Ref PublicRouteTable1
			DestinationCidrBlock: 0.0.0.0/0
			GatewayId: !Ref InternetGateway1
	PublicSubnet1RouteTableAssociation1:
		Type: "AWS::EC2::SubnetRouteTableAssociation"
		Properties:
			SubnetId: !Ref VPC1PublicSubnet1
			RouteTableId: !Ref PublicRouteTable1
	PublicSubnet1RouteTableAssociation2:
		Type: "AWS::EC2::SubnetRouteTableAssociation"
		Properties:
			SubnetId: !Ref VPC1PublicSubnet2
			RouteTableId: !Ref PublicRouteTable1
	PublicNetworkAcl1:
		Type: "AWS::EC2::NetworkAcl"
		Properties:
			VpcId: !Ref VPC1
			Tags:
				- Key: Name
				  Value: ACG_NACL
	InboundHTTPPublicNetworkAcl1Entry:
		Type: "AWS::EC2::NetworkAclEntry"
		Properties:
			NetworkAclId: !Ref PublicNetworkAcl1
			RuleNumber: "100"
			Protocol: "6"
			RuleAction: allow
			Egress: "false"
			CidrBlock: 0.0.0.0/0
			PortRange:
				From: "80"
				To: "80"
	InboundHTTPSPublicNetworkAcl1Entry:
		Type: "AWS::EC2::NetworkAclEntry"
		Properties:
			NetworkAclId: !Ref PublicNetworkAcl1
			RuleNumber: "101"
			Protocol: "6"
			RuleAction: allow
			Egress: "false"
			CidrBlock: 0.0.0.0/0
			PortRange:
				From: "443"
				To: "443"
	InboundSSHPublicNetworkAcl1Entry:
		Type: "AWS::EC2::NetworkAclEntry"
		Properties:
			NetworkAclId: !Ref PublicNetworkAcl1
			RuleNumber: "102"
			Protocol: "6"
			RuleAction: allow
			Egress: "false"
			CidrBlock: 0.0.0.0/0
			PortRange:
				From: "22"
				To: "22"
	InboundEmphemeralPublicNetworkAcl1Entry:
		Type: "AWS::EC2::NetworkAclEntry"
		Properties:
			NetworkAclId: !Ref PublicNetworkAcl1
			RuleNumber: "103"
			Protocol: "6"
			RuleAction: allow
			Egress: "false"
			CidrBlock: 0.0.0.0/0
			PortRange:
				From: "1024"
				To: "65535"
	OutboundPublicNetworkAcl1Entry:
		Type: "AWS::EC2::NetworkAclEntry"
		Properties:
			NetworkAclId: !Ref PublicNetworkAcl1
			RuleNumber: "100"
			Protocol: "6"
			RuleAction: allow
			Egress: "true"
			CidrBlock: 0.0.0.0/0
			PortRange:
				From: "0"
				To: "65535"
	PublicSubnet1NetworkAclAssociation1:
		Type: "AWS::EC2::SubnetNetworkAclAssociation"
		Properties:
			SubnetId: !Ref VPC1PublicSubnet1
			NetworkAclId: !Ref PublicNetworkAcl1
	PublicSubnet1NetworkAclAssociation2:
		Type: "AWS::EC2::SubnetNetworkAclAssociation"
		Properties:
			SubnetId: !Ref VPC1PublicSubnet2
			NetworkAclId: !Ref PublicNetworkAcl1
	EC2Instance1:
		Type: "AWS::EC2::Instance"
		Properties:
			InstanceType: !Ref InstanceTypeParameter
			ImageId: "ami-0b72821e2f351e396"
		UserData:
			Fn::Base64: !Sub |
				#!/bin/bash
				'echo ''cloud_user:%password%'' | chpasswd'
				yum install -y httpd
				systemctl start httpd
				systemctl enable httpd
				TOKEN=$(curl --request PUT "http://169.254.169.254/latest/api/token" --header "X-aws-ec2-metadata-token-ttl-seconds: 3600")
				INSTANCE_METADATA==$(curl -s http://169.254.169.254/latest/meta-data/ --header "X-aws-ec2-metadata-token: $TOKEN")
				INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id --header "X-aws-ec2-metadata-token: $TOKEN")
				PUBLIC_IP=$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4 --header "X-aws-ec2-metadata-token: $TOKEN")
				LOCAL_IP=$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4 --header "X-aws-ec2-metadata-token: $TOKEN")
				AZ=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone --header "X-aws-ec2-metadata-token: $TOKEN")
				echo "<html><h1>Instance Data</h1><h3>Availability Zone: " $AZ > /var/www/html/index.html
				echo "</h3><h3>Public IP: " $PUBLIC_IP >> /var/www/html/index.html
				echo "</h3><h3>Local IP: " $LOCAL_IP >> /var/www/html/index.html
				echo "</h3><h3>Instance Id: " $INSTANCE_ID >> /var/www/html/index.html
				echo 'setup done.' > /home/cloud_user/status
				'chown cloud_user:cloud_user /home/cloud_user/status'
		NetworkInterfaces:
			- GroupSet:
				- !Ref EC2SecurityGroup1
			  AssociatePublicIpAddress: "true"
			  DeviceIndex: "0"
			  DeleteOnTermination: "true"
			  SubnetId: !Ref VPC1PublicSubnet1
		Tags:
			- Key: Name
			  Value: MyInstance
	EC2SecurityGroup1:
		Type: "AWS::EC2::SecurityGroup"
		Properties:
			GroupDescription: EC2SecurityGroupPublic
			VpcId: !Ref VPC1
	SecurityGroupIngress:
		- CidrIp: 0.0.0.0/0
		  FromPort: 22
		  ToPort: 22
		  IpProtocol: tcp
		- CidrIp: 0.0.0.0/0
		  FromPort: 80
		  ToPort: 80
		  IpProtocol: tcp
		GroupName: ACG_SG
		Tags:
			- Key: Name
			  Value: ACG_SG
			- Key: Test
			  Value: MyInstanceTest
```


### DeletionPolicy 
控制用户不能随便删除资源

可以用AWS Service Catalog 分享 Portfolio的方法, 分享基础设施, 环境的构建方案
