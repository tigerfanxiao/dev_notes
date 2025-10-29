
# 理论

FTP需要建立管理通信和数据通信
管理通信都是由客户端向服务端发起的TCP连接. 
数据通信根据服务端的行为分为主动模式和被动模式

在客户端向服务端发起TCP请求后, 建立管理通信

主动模式: 服务器端向客户端发起数据传输的TCP请求
被动模式: 服务器端制定客户端数据传输使用的端口, 再由客户端使用这个端口向服务端发起TCP请求. 

```shell

# server 端
ftp server enable
set default ftp-directory flash:
aaa
local-user huawei password cipher huawei
local-user huawei idle-timeout 5 0
local-user huawei privilege level 15
local-user huawei ftp-directory flash:
local-user huawei service-type ftp
save


dir flash: # 查看文件夹
rename flash:/vrpcfg.zip flash:/r2.zip  # 重命名文件
display startup # 查看启动参数
startup saved-configuration r2.zip # 设置下一次启动的配置文件
delete flash :/r1.zip # 删除文件放入回收站
```


FTP
```shell
# 建立FTP连接
# windows
open 12.2.2.1

# huawei device
ftp 12.2.2.1


# FTP 命令
put r1.zip # 上传文件
get flash:/r1.zip# 下载文件


```

acl控制
```shell

acl number 3000
rule 5 permit icmp
rule 10 permit tcp destination-port eq ftp  # 这个是控制 21
rule 15 permit tcp destination-port eq ftp-data # 这个是数据端口 20
rule 1000 deny ip
```
