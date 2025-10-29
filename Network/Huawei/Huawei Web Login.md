通过浏览器访问方式来上传设备新的版本是比较快的

首先通过 console 界面选一个接口配置 IP 地址
然后在自己笔记本上网口配置一个同一个网段的 IP 地址
测试是否可以 ping 通 Huawei 的交换机的接口


```shell
# 打开 web 访问功能
http secure-server enable 
# 所有的接口都可以访问
http server-source all-interface 

# 需要创建一个用户
local-user admin password irreversible-cipher privilege level 15
local-user admin service-type web ftp http ssh terminal
# 用户登陆后, 不需要修改密码
undo local-aaa-user password policy administrato

```

