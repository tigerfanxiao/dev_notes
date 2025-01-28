
Linux的用户分User和Group
Linux给每一个用户都会分配一个ID， 也成为UID
UID的范围 
0 是superuser root用户
1-200 是系统进程
201-999 没有任何文件的系统进程
1000+ 普通用户

对于Group来说也有GID的概念



```shell

root	ALL=(ALL:ALL) ALL
newuser  ALL=(ALL:ALL) ALL
```