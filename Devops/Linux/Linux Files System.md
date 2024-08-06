
# File Directory
```shell
/bin # 存储 cd, mv 等命令的的二进制文件, 比如 ls 命令, 就是/bin/ls 文件
/sbin # 系统管理员使用的命令的二进制文件
/usr  # used for sharing files between users
/media # usb 介质
/home # 所有用户的文件夹都放在 home 下面
/etc # 程序使用的配置文件
/boot # 存放开机启动需要的程序
```


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

# 文件操作

```shell
ls -al # 查询当前目录下所有文件包括隐藏的

# 通配符
ls fil? # ? 表示单个字符
ls f* # 匹配任意长字符
ls file[1-9] # 匹配 file1, file2, file3, 只能匹配单个数字
ls *[[:digit:]] # 单个数字


ls | grep -E '^file' # 查询所有 file 开头的文件

```

```shell
# 创建1000 个文件
for i in {1..1000}; do touch file_$i; done

# 删除所有文件
rm file_[1-1000]
rm file_*
```

# Media
### USB Driver

```shell
lsblk # 查看当前的 usb 设备
```