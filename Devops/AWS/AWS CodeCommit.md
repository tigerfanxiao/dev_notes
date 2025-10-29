Private Repo managed service on AWS
## Permissions
- 需要给 CodeCommit 的用户生成登录的AK/SK, 在 IAM 中操作


```shell
# AWS Cli command to create repo on codecommit
aws codecommit create-repository --repository-name <repository-name> --repository-description "<description>"


# list all repos
aws codecommit list-repositories

# view repos info
aws codecommit get-repository --repository-name <repository-name>

# delete repo
aws codecommit delete-repository --repository-name <repository-name>
```


## Features
防止代码中包含密码信息被上传. Code Commit可以设置trigger, 关联lambda函数去检查上传的代码