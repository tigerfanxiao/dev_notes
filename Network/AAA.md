定义: 验证, 授权和计费
Authentication, Authorization, Accounting

# 概念

### 域
每一个域有一套独立的认证, 授权计费的的策略. 相同的用户可以加入一个域



认证方式有三种
1. 不认证
2. 本地认证 local-user
3. 远端认证

组件
1. 用户
2. NAS
3. AAA服务器

认证过程
1. 用户发送用户名和密码给NAS
2. NAS收到用户名和密码后, 确定该用户属于哪一个域, 发送认证请求给AAA服务器
3. AAA服务器完成认证, 开始计费


认证协议有hwtacas, radius(使用较多)

NAS 并不是认证服务器, 它不做认证, 只是判断用户属于哪一个域. 
每个域可以有不同认证方式, 指向不同的认证服务器


配置域
 
```shell
# 新建一个域

domain 211
authentication-scheme 123


# 新建一个用户放入211域
local-user username@211 password cipher huawei 

```