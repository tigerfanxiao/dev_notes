
# Concepts

账户有两种
- 一种是 root user, 要用邮箱登录, 平时操作不要使用
- IAM user 

3A
- Authentication simply answers the question, “Are you who you say you are?”
- Authorization answers the question, “What actions can you perform?”

AWS Root User have two set of credential
- Email/Password
- AK/SK used for API

Best practice for root user
- Disable or delete the access keys associated with the root user. 现在默认不创建了. 可以在My Security Credential 中修改
- 使用 MFA
- 创建一个 IAM User

### Policy
Policy 有 AWS 在维护的, 也有用户自己定义的
Policy 不能单独使用, 需要 attach 到 Group 或者 User 上
Policy 怎么组成
- Sid
- Effect: Allow 还是 Deny
- Action: 资源的 API
- Resource 

### Role Based Access in AWS
IAM Roles
- 如果是 APP 需要调用 S3, 不建议给 APP 创建一个User, 而是给 APP 一个 IAM Role
- IAM Role 不需要设置静态的密码, 而是用程序的方式给一个临时的密钥. 这个密钥会自动 rotate
- 如果是公司外部有 identity provider (IdP), 也是建议创建 IAM Role 可以使用 AWS IAM Identity Center (Successor to AWS Single Sign-On )来完成身份认证