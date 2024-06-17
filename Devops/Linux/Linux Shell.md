
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

grep # return lines in file matching pattern
find # find files in directory tree
ls
pwd
mkdir
cd
rmdir # remove directory
cat
more # print file page by page
head -3 # print first N lines
tail -3 <file_name> # print last 10 lines
tail -f <file_name> # 查看一直在更新的文件末尾
echo # print string or variable 
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

### `more` & `less`
more 和 less 的区别在于. more 是一个 page 一个 page 翻的, 而且不能往回翻. less 可以一行一行翻, 用 page up 和 page down, 而且可以往回翻
### `wc`
```shell
wc pets.txt # count lines, word of file
7 7 28 pets.txt # 7 lines, 7 words, 28 characters
wc -l pets.txt # return how many lines
wc -w pets.txt # word count
wc -c pets.txt # character count
```

### `sort`
```shell

sort pets.txt # 把每一行按照首字母进行排序
sort -r pets.txt # 逆向排序

```

### `uniq`
```shell
uniq pets.txt # 这是去除连续的重复的行
```

### `grep`
```shell
grep ch pets.txt # 在 pets.txt 文件中查找有 ch 字符的行
grep -i ch petx.txt # 不区分大小写
grep "not installed" /var/log/bootstrap.log # 查询字符串
grep not installed /var/log/bootstrap.log # 查询包含 not 或者包含 installed

-n # Along with the matching lines, also print the line numbers|
-c # Get the count of matching lines|
-i # Ignore the case of the text while matching|
-v # Print all lines which do not contain the pattern|
-w # Match only if the pattern matches whole words|


-o # tells `grep` to _only_ return the matching portion
-E # tells `grep` to be able to use extended regex symbols such as `?`
\"price\" # matches the string `"price"`
\s* # matches any number (including 0) of whitespace (`\s`) characters
[0-9]* # matches any number of digits (from 0 to 9)
?\. # optionally matches a `.` 小数点
```


### `cut`

```shell
cut -c 2-9 people.txt # 从 people.txt 文件中抽取第 2 列到第9 列的内容
cut -c -2 zoo.txt # 从 zoo.txt 中取出前 2 个字符
cut -c -2- zoo.txt # 从 zoo.txt 取出第二个字符后面所有的字符
cut -d ' ' -f2 people.txt # 抽取文件中第二列的内容, 列的分隔符是空格
```

### `paste`
```shell
paste a.txt b.txt c.txt # 横向多个列合并, tab 作为分割符
paste -d "," a.txt b.txt # 两个文件合并的时候, 用逗号分割

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


date -r <file_name> # 查看文件的日期
```

```shell
# 不显示命令回显
command > /dev/null
```

### `ls`
```shell
ls -d # 只显示文件件
ls -h # 文件大小
ls -S # sort by size
ls -t # sort by time
ls -r # reverse the sort order
ls -a # all files include hidden
ls -l # all information include permission
```

### `find`
```shell
find . -name <file_name> # 在当前目录下寻找
find . -iname <file_name> # 不区分大小写寻找


```
### `rm`

```shell
rmdir <dir> # 删除目录
rm -r <dir> # 删除目录及下面的文件

```

### `chmod`
```shell
chmod +x <file.sh> # 给bash脚本增加执行权限
chmod go-r my_new_file # remove group and other read permission

# 目录, s 表示所有在这个文件夹上创建的文件都有相同的 group 权限
# 目录的执行权限, 表示可以进入这个目录
# 目录的写权限, 表示在目录下创建文件夹或者文件
drwxr-sr-x 2 theia users 4096 Jun 17 07:27 test
```

### `cp`
```shell
cp -r <dir/> <dir/>  # 把目录下的文件 copy 到另一个目录下

```

### `curl`

```shell
# downlaod file and save as
curl -o top-sites.txt https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/top-sites.txt

# save to current directory with the default file name
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt


# API 调用, 返回 JSON 文件
curl --request GET \
     --url https://openapiv1.coinstats.app/coins \
     --header 'X-API-KEY: ZufUoOV21Q7FuKb9HLM/UfJ5EgRNjp0tTm7w6q3kO/4=' \
     --header 'accept: application/json'

```

### `tar`
```shell
tar -cf note.tar notes # 把 notes 文件夹打包成 note.tar
tar -czf note.tar.gz ntoes # 把 notes 文件夹打包压缩成 note.tar.gz

tar -tf note.tar # 查看包中文件结构

tar -xf note.tar notes # 把包 note.tar 解开到 notes
tar -xzf note.tar.gz notes # 把包 note.tar.gz 解压到 notes 中
```

### `zip`
```shell

zip -r notes.zip notes # 把 notes 文件夹直接压缩成 notes.zip 文件
zip config.zip /etc/*.conf # 把所有的 config 文件都压缩到 config.zip 文件
unzip notes.zip # 解压 zip 文件

zip -l notes.zip # 查看 zip 文件中的结构
unzip -o bin.zip # 解压文件 -o 表示 overwrite


```
# Bash Script

bash 脚本基本格式
```shell
#! /bin/bash 

echo "Hello Network!"
```