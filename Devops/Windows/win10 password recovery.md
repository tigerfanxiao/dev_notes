
按住 shift 键重启, 进入安全模式
选择命令行

```shell

diskpart
list volume # 查看磁盘分区, 查看系统盘符的名字, 比如是 c 盘
exit # 退出分区工具 

c: # 进入 c 盘
dir # 查看根目录, 是否有 Windows 目录
cd Windows
cd system32

ren utilman.exe utilman1.exe # 修改文件名
ren cmd.exe utilman.exe # 修改文件名

# 关闭cmd 窗口, 回到安全模式
# 选择继续启动 windows
# 进入 windows 登录界面后, 选择右下角辅助功能, 打开命令行
net user # 查看用户列表
net user admin * # 这里 admin 是需要恢复密码用户名
# 重新输入用户的密码
# 关闭窗口
# 在登录页面重新输入新设置的密码, 此时可以登录windows

# 再次重新启动进入安全模式
# 把之前修改的文件名改回来
c: 
dir 
cd Windows
cd system32
ren utilman.exe cmd.exe  
ren utilman1.exe utilman.exe
# 点击继续登录
```