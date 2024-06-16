
```shell
/bin # 存储 cd, mv 等命令的的二进制文件, 比如 ls 命令, 就是/bin/ls 文件
/sbin # 系统管理员使用的命令的二进制文件
/usr  # used for sharing files between users
/media # usb 介质
/home # 有多少个普通用户, 下面就有多好个文件夹

/boot # 存放开机启动需要的程序
```

Linux 文件权限有三个维度, 使用 `chmod`修改
- user是文件的所有者
- group 是组
- other 为其他

Linux 文件的所有权有两个维度. 使用`chown`修改
- user
- 组
### 文件权限

```shell
-rw-rw-r-- # 三个组 User Group Other
```



```shell
chmod u+x filename.txt # 给 user 添加执行权限
chmod go+x filename.txt # 给 group 和 other 增加执行权限
chmod ugo-wr <filename> # 删除 user, group, other 的 write, read 权限
chmod +x <filename> # 给文件所有的组加上执行权限
```


# USB Driver

```shell
lsblk # 查看当前的 usb 设备

```