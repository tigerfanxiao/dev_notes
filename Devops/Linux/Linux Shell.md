
# Common Commands
```shell
printenv SHELL # 查看当前适用的是什么 shell
bash # 切换 bash
whoami # 查看当前用户
id # 当前用户id 和组 ID
unane # 当前系统名称
ps # running process
top # running process and resource usage, cpu, memory, io
df # mounted file system 
man # reference manuel
date # print date
cp # copy file
mv # move file
rm # remove file
touch # create empty file, update file timestamp
chmod # change file permission
wc # count lines, word of file
grep # return lines in file matching pattern
find # find files in directory tree
ls
pwd
mkdir
cd
rmdir # remove directory
cat
more # print file page by page
head # print first N lines 
tail # print last N lines
echo # print string or variable 
tar # archive a set of files
zip # compress a set of files
unzip # extract files from a compressed zip archive
gzip
# network
hostname 
ping 
ifconfig # 需要安装 nettools
curl # display contents of file at a URL, 类似浏览器答应出 html
wget # download file from URL

```

### `id`
```shell
id -u # id of user
id -u -n # the name of user = whoami 
```
### `uname`
```shell
uname -s -r # kernal vesion
uname -v # verbol version
```
### `df`
```shell
df -h # human readable disk usage
```

### `ps`
```shell
ps # 查看当前用户运行的进程
ps -e # 查看所有用户运行的进程
```
### `top`
默认是按照 cpu usage 排序的
```shell
top -n 3 # show top 3 running process
# 排序
shift + m # memory
shift + p # CPU Usage
shift + n # Process ID (PID)
shift + t # Running Time
```

### `echo`

```shell
echo $PATH
# 没有 -e 则不会换行打印
echo -e "Hello! \nGoodbye!" # \n 之前必须要有空格

```

### `date`
```shell
date "+It's %A, %j day of %Y"
# It's Friday, the 097 day of 2023
date "+%T"

%d # Displays the day of the month (01 to 31)
%h # Displays the abbreviated month name (Jan to Dec)
%m # Displays the month of year (01 to 12)
%Y # Displays the four-digit year
%T # Displays the time in 24 hour format as HH:MM:SS
%H # Displays the hour
```

```shell
# 不显示命令回显
command > /dev/null
```

# Bash Script

bash 脚本基本格式
```shell
#! /bin/bash 

echo "Hello Network!"
```