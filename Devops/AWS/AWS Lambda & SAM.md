将本地仓库代码 SAM化
- 首先构建template.yaml 和 samconfig.toml 两个文件
- 通过uv 重新构建 requirements.txt

```
Project_dir/
│
├── template.yaml          ← SAM template
├── samconfig.toml         ← Generated after first deployment
├── pyproject.toml
├── uv.lock
├── README.md
├── .gitignore
│
├── src/
│   ├── handler.py
│   ├── services/
│   │   ├── pipeline_service.py
│   │   ├── athena_service.py
│   │   ├── bedrock_service.py
│   │   └── ...
│   ├── models/
│   └── config/
│
├── events/
│   └── event.json
│
└── tests/
```

template.yaml 的例子, 使用 `sam validate` 验证
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: AI-Sandbox-pdp-auditor
Resources:
	AthenaBedrockFunction:
		Type: AWS::Serverless::Function
		Properties:
			CodeUri: src/
			Handler: handler.lambda_handler # Point to src/hander.py
			Runtime: python3.13
			Architectures:
				- arm64
			Timeout: 900
			MemorySize: 2048
```

区分 dev和prod 两个stage, samconfig.toml
```toml
version = 0.1

[dev.deploy.parameters]
stack_name = "ai-sandbox-pdp-auditor-dev"
region = "eu-west-1"
resolve_s3 = true
s3_prefix = "ai-sandbox-pdp-auditor-dev"
capabilities = "CAPABILITY_IAM"
confirm_changeset = true
parameter_overrides = "Environment=dev"

[prod.deploy.parameters]
stack_name = "ai-sandbox-pdp-auditor-prod"
region = "eu-west-1"
resolve_s3 = true
s3_prefix = "ai-sandbox-pdp-auditor-prod"
capabilities = "CAPABILITY_IAM"
confirm_changeset = true
parameter_overrides = "Environment=prod"
```
deploy 命令
````shell
sam deploy --config-env dev
````
本地运行
```shell
sam local invoke AthenaBedrockFunction
# 每次修改 template.yaml 都需要删除 .aws_sam

rm -rf .aws-sam
sam build 
sam build --use-container

```

```shell
sam build --use-container \
  --container-env-var PIP_EXTRA_INDEX_URL="$PIP_EXTRA_INDEX_URL"
```

```shell
uv export --format requirements-txt --no-dev > requirements.txt
```

```shell
aws dynamodb export-table-to-point-in-time \
  --table-arn arn:aws:dynamodb:eu-west-1:$(aws sts get-caller-identity --query Account --output text):table/ai-sandbox-pdp-auditor-results-dev \
  --s3-bucket ai-sandbox-pdp-auditor-exports-dev \
  --s3-prefix exports \
  --export-format ION \
  --region eu-west-1
  

{
    "ExportDescription": {
        "ExportArn": "arn:aws:dynamodb:eu-west-1:757992231822:table/ai-sandbox-pdp-auditor-results-dev/export/01786011628183-58d21314",
        "ExportStatus": "IN_PROGRESS",
        "StartTime": "2026-08-06T12:20:28.183000+02:00",
        "TableArn": "arn:aws:dynamodb:eu-west-1:757992231822:table/ai-sandbox-pdp-auditor-results-dev",
        "TableId": "b82bb46b-9e62-4b11-b866-2ac01078d92f",
        "ExportTime": "2026-08-06T12:20:28.183000+02:00",
        "ClientToken": "da3985e0-83bf-4623-b7ff-6607aee0a49e",
        "S3Bucket": "ai-sandbox-pdp-auditor-exports-dev",
        "S3Prefix": "exports",
        "S3SseAlgorithm": "AES256",
        "ExportFormat": "ION"
    }
}

aws dynamodb describe-export \
  --export-arn "arn:aws:dynamodb:eu-west-1:757992231822:table/ai-sandbox-pdp-auditor-results-dev/export/01786011628183-58d21314" \
  --region eu-west-1 \
  --query 'ExportDescription.{Status:ExportStatus, StartTime:StartTime, EndTime:EndTime, S3Bucket:S3Bucket, S3Prefix:S3Prefix}'
```


