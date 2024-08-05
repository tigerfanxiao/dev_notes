
```shell
# 查询当前运行的所有进程
ps aux 

# 查询属于cloud_user的进程
ps -U cloud_user 

# 查询某个进程有多少线程
cat /proc/<PID>/status | grep Thread 
```