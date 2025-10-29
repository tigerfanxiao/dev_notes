```shell
# 用默认的文本编辑器打开 crontab 文件
crontab -e

crontab -l | tail -6 # 查看所有任务
crontab -r # 删除所有的任务

sudo service crontab start
sudo service crontab stop
```

```shell
# minute hour dayofmonth month dayofweek
m h dom mom dow command
# 每周日 15 点 30 分, 执行命令 
30 15 * * 0 date >> sundays.txt 
# 每分钟
* * * * * date >> /tmp/everymin.txt
```

定时任务不执行
查看任务执行文件的 user, 当前用户是否有执行权限