
# Steps

1. 生成RSA 秘钥对
```shell
rsa local-key-pair create # 后面选 1024 bit
```
2. 创建账户
```shell
aaa
local-user your_username privilege level 15 password cipher Huawei@123
# 或者
local-user your_username privilge level 15 irreversible-cipher Huawei@123
# 注意这里 irreversible-cipher 是不可逆的密码
local-user your_username service-type ssh
quit

```
3. 启动ssh server
```shell
stelnet server enable
```

4. 设置vty验证模式是aaa, 访问方式是SSH
```shell
user-interface vty 0 4
authentication-mode aaa
protocol inbound ssh
quit

```

5. 配置ssh属性
```shell
ssh user your_username authentication-type password # 配置用户ssh认证为密码
ssh user service-type stelnet # 配置ssh服务类型为stelent
ssh server port 22622 # 配置ssh登陆端口为22622
```

### 其他用户访问指定设备

```shell
# 第一次
ssh client first-time enable 
# 在system-view模式下
stelnet 192.168.1.1
# 输入用户名和密码
```

查看用户和vty状态命令
```shell
dis users # 查看当前登录的用户名
dis user-interface # 查看当前用户权限

```