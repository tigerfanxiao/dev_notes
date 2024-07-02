
```shell

aaa
local-user admin password irre <password> privilege 15
# 用户可以访问的协议
local-user admin service-type web http ftp
# 登录后不需要更换密码
undo local-aaa-user password policy administrato

```