

### 打印列

```shell

cloud_user@ip-10-0-1-10:~$ ps
  PID TTY          TIME CMD
 3845 pts/0    00:00:00 bash
 3939 pts/0    00:00:00 ps

# 打印第一列
cloud_user@ip-10-0-1-10:~$ ps | awk '{print $1}'
PID
3845
3941
3942

# 打印第二列
cloud_user@ip-10-0-1-10:~$ ps | awk '{print $2}'
TTY
pts/0
pts/0
pts/0


```