Linux 文件分为 3 个组
- user是文件的所有者
- group 是组
- other 为其他
### 文件权限


```shell
-rw-rw-r-- # 三个组 User Group Other
```

```shell
chmod u+x filename.txt # 给 user 添加执行权限
chmod go+x filename.txt # 给 group 和 other 增加执行权限
```
