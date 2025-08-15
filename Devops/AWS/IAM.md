对象
* User 新用户没有权限
* Roles 
* Groups
* Policies

Roles和 Groups 的区别
Groups 里面都是人, Roles 里面可以是某个服务


最佳实践
访问数据库
1. 把数据库的密码放在 AWS Secret Manager里
2. 创建一个role
3. 其他资源可以通过assume这个role来访问数据库
## root account
在创建 AWS 账户时, 就是 root account, 这个账户最好 MFA 认证
