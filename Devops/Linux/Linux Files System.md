
# File Directory
- `/bin`和 `/sbin` 的区别是, `/bin` 是普通用户可以还行, `/sbin` 是管理员才能执行的程序
- linux 内核文件保存在 `/boot`下, 只有 12M
```shell
/bin # 存储 cd, mv 等命令的的二进制文件, 比如 ls 命令, 就是/bin/ls 文件
/sbin # 系统管理员使用的命令的二进制文件
/usr  # used for sharing files between users
/media # usb 介质
/home # 所有用户的文件夹都放在 home 下面
/etc # 相当于 windows 中的注册表, 存储 linux 所有程序的配置文件
/boot # 存放开机启动需要的程序, linux 内核也在里面
/run # 程序运行的时候的临时文件
/tmp # 用户和程序的临时文件
/var # 日志等文件
/proc # 吧保存在内存中的文件, 用于进程
/sys # 保存在内存中的文件, 用于硬件
```

`/dev`下有块设备, 字符设备
块设备, 比如硬盘. 一个块包含多个字节
字符设备, 一个字符一个字符为单位

### File Privilege
Linux 文件权限有三个维度, 使用 `chmod`修改
- user是文件的所有者
- group 是组
- other 为其他

```shell
-rw-rw-r-- # 三个组 User Group Other
```

修改文件权限
```shell
chmod u+x filename.txt # 给 user 添加执行权限
chmod go+x filename.txt # 给 group 和 other 增加执行权限
chmod ugo-wr <filename> # 删除 user, group, other 的 write, read 权限
chmod +x <filename> # 给文件所有的组加上执行权限
```


Linux 文件的所有权有两个维度. 使用`chown`修改
- user
- 组

