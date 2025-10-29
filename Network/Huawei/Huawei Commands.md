# SSH Connection

```shell
# 创建账户名, 权限登记和对应服务
aaa
  local-user <username> password irre <password> 
  local-user <username> privilege level 15
  local-user <username> service-type terminal ssh telnet
  local-aaa-user password policy admini
     undo password alert original # 第一次不用修改密码
     undo password expire # 密码不过期

# 开启 stelnet 服务
stelnet server enable 

# 配置ssh
ssh user <username> authe password # ssh用户用密码登录
ssh user <username> service stelnet # ssh用户使用 stelnet 协议
ssh server-source all # 所有接口都开启接受 ssh 连接请求
ssh client first-time enable
undo ssh server publickey # 不使用public rsa key来登录

# 外部连接
user-interface vty 0 4
 acl 3100 inbound # 做限源
 authentication-mode aaa
 user privilege level 15
 protocol inbound all

acl number 3100
 rule 10 permit ip source 192.168.100.1 0 
 rule 15 permit ip source 192.168.200.2 0 
 rule 20 permit ip source 192.168.200.3 0 
 rule 25 permit ip source 192.168.30.0 0.0.0.255 (5778 matches)
 rule 30 permit ip source 140.205.85.128 0.0.0.63 (28 matches)
 rule 35 permit ip source 94.158.254.5 0 (40 matches) 
 rule 40 permit ip source 94.158.254.2 0 
```
# Console 
```shell
# 所有 aaa 中创建的账户都可以使用 console 登录
user-interface con 0
authentication-mode aaa
```