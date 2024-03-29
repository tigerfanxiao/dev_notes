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
