# How to install aws-cli
## Install aws-cli
### Install aws-cli through powershell
```powershell
Invoke-WebRequest -Uri "https://awscli.amazonaws.com/AWSCLIV2.msi" -OutFile "AWSCLIV2.msi"

Start-Process -Wait -FilePath ".\AWSCLIV2.msi"

# 重启powershell之后
aws --version # return the following info
aws-cli/2.12.7 Python/3.11.4 Darwin/23.6.0 source/arm64 prompt/off

```
### Install aws-cli on mac
```shell
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
# check if aws cli installed successfully
aws --version
```
## config user profile
```shell
# in cmd line
aws configure --profile <user_name>
aws_access_key_id = <AK>
aws_secret_access_key = <SK>
Default region name [None]: us-east-1
Default output format [None]: json
```
config file
```shell
[profile xiao]
region = eu-west-1
output = json
```

# credencial

首先安装 aws cli 后, boto3 会从 `~/.aws/credentials`读取密码. 注意, 只会读取 default 的密码

```
[default] 
aws_access_key_id = <AK>
aws_secret_access_key = <SK>
```

```shell
# 创建S3 bucket
aws s3api create-bucket --bucket <bucket_name> --profile <user_name>
{
    "Location": "/255050296125demobucket"
}
```

