存储访问数据库的密码. 可以定期自动更新密码


```python
# 通过secretId来获取密码
secret_password_id = "my-secret-password"
region = "us-east-1"

session = boto3.session.Session()
client = session.client(
	service_name = "secretsmanager",
	region_name = region,
)

def lambda_hander(event, context):
	get_secret_password = client.get_secret_value(
		SecretId = secret_password_id,
	)
```