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
uv export --format requirements-txt --no-dev > src/requirements.txt
```