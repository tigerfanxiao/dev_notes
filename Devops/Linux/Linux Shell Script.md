# Shell 脚本格式
- 脚本在执行时, 如果中间有命令执行错误(语法除外), 还是能够继续执行下去的
shebang
- 所有的解释性语言, 首行都要写 shebang, 比如python
在 linux 中运行的 python 脚本需要用
```shell
#!/usr/bin/env python3
# -*- coding=utf-8 -*-
```

在 linux 中运行的 Bash shell 脚本
```shell
#!/bin/bash
```

### 执行脚本的方法
```shell
# 1. 加执行权限
chmod u+x helloworld.sh
./helloworld.sh

# 2. 如果文件没有执行权限, 可以通过命令来执行
bash helloworld.sh

# 3. bash 接受标准输入
echo hostname | bash 

# 4. 把脚本放在服务器上, 下载直接运行
curl <url.sh> 2>/dev/null | bash # 把标准输出传给了 bash

```

# 脚本
### 算数运算
- shell不支持浮点数运算
```shell
# 需要使用 let
let k = i + j 
((k=i+j)) # 或者这种写法
k=$[i-j] # 或者这种写法
# 打印结果
echo $k
# 使用 expr 表达式
expr 10 + 20
echo 10+20 | bc

# random 变量 在 0-32767之间. 是 bash 内置的变量
echo $RANDOM 
echo $[RANDOM%80+1] # 1-80之间
COLOR=$[RANDOM%7+31] # 31-37之间的 7 中颜色

# 自增, 注意 i 还是要自增的
i=2
let i++
let ++i

# 二进制与运算
echo $[4&5]
# 取反
echo $[!2]
! true # fals5e
! false
# 异或
x=10;y=20;x=$[x^y];y=$[x^y];x=$[x^y];echo x=$x y=$y

```

# Shell 进程
- `. test.sh` 这种用法是不推荐的. 因为这个脚本不会独立开启一个子进程, 而是直接在当前的bash进程中运行. 如果此时bash 进程有同名的变量, 就会发生变量替换
- 如果是配置文件, 确实应该用 `. .bashrc` 因为就是要配置文件在当前 bash 进程中生效
- 管道符后面也会创建一个子进程
```shell
# 在()中的子进程可以继承父进程, 但是只在子进程中有效
name=magedu;(echo $name;name=49;echo $name;);echo $name
# magedu m49 magedu

cd /data; umask 666; touch app.key
```
# 错误提示
- 错误有三种: 语法错误, 命令错误, 逻辑错误
- 往往提示可以发现执行到发生错误的地方. 提示有 Syntax Error
- 命令错误会继续执行. 比如某一条命令不存在
```shell
 # 扫描脚本中的语法错误, 但是不会执行
 # 无法发现命令错误
bash -n ./system_info.sh

# 排查命令错误
bash -x ./system_info.sh # 跟踪每一个命令执行的过程

cat -A  test.sh # 查看看不见的字符, 特别是 EOF 后面有没有空格
cat -n 18 test.sh # 查看对应的行号的
vim 中 输入 :set list # 查看有没有不可见的空格错误
```
# 状态码
```shell

echo $? # 成功是 0

# 自定义返回结果
exit 100 # 最大支持 255
exit # 提前退出程序 


IP=1.0.0.100; ping -c1 -W1 $IP &> /dev/null && echo "$IP is up" || { echo "$IP is unreachable"; exit;}

# 查看脚本运行时间
time bash <file.sh>
```
# 配置文件
全局配置文件
``` shell
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
```
个人配置文件
```shell
~/.bash_profile
~/.bashrc
```
交互式shell 登录配置文件执行顺序
```shell
# 放在每个文件最前
/etc/profile  # 放环境变量, 命令和脚本
/etc/profile.d/*.sh
/etc/bashrc # 放别名和函数, 或者环境变量
~/.bash_profile
~/.bashrc
/etc/bashrc # 又执行一遍

# 放在每个文件最后
/etc/profile.d/*.sh
/etc/bashrc
/etc/profile
/etc/bashrc # 又执行一遍
~/.bashrc
~/.bash_profile
```
非交互登录, 即使用 su 切换
```shell
# 执行次序
/etc/profile.d/*.sh
/etc/bashrc
~/.bashrc
```

# Shell Variable 
在 shell 中定义变量
```shell
firstname="Jeff" # 注意这里等号左右没有空格
echo $firstname

read lastname
Grossman # 输入给变量赋值
```
shell 本身的 variable
```shell
set | head -4 # 查看 shell 中定义的所有变量
unset firstname # delete the variable firstname

# 把 shell variable 提升为环境变量
export var_name 
declare -x var_name # 也可以用来声明环境变量
# 查看所有的环境变量
env 

# 查看某个进程中的环境变量
cat /proc/10710/environ

# 常量
readonly PI=3.1415926 # 这种方法定义的变量, 不能改动
# 显示所有的常量
readonly

# 位置变量
./myshell.sh arg1 arg2
$1 # 在脚本后的第一个字符串
${10} # 第 10 个参数
$0 # 代表脚本本身
$* # 所有参数
$@ # 所有参数
$# # 参数个数


```


获取用户输入
```shell
#! /bin/bash
echo -n "Enter your name :"	  	
read name # 声明一个变量, 把用户的输入存放在变量中

# Print the welcome message followed by the name	
echo "Welcome $name" 

# The following message should print on a single line. Hence the usage of \'-n\'
echo -n "Congratulations! You just created and ran your first shell script "
echo "using Bash on IBM Skills Network"
```

command substitution
- 当有很多文件公用相同的变量. 可以写一个变量的文件. 在别的脚本中调用这个文件就行
```shell
# 有两种方法
$(command)
`command`

here=$(pwd) # 把命令的结果保存到变量中

nums=`echo {1..10}` # 命令的结果赋值给变量

# 在数学运算中需要用(())
sum=$(($n1+$n2))

# 定义多行变量 
course ="
> Linux
> golong
> python
"
echo "$course" # 保持原格式打印变量
echo $course # 不保持格式打印变量

# 在脚本中直接调用脚本, 在当前进程中有效. 就可以直接引用脚本中定义的变量
. /etc/os-release 
# 删除变量
unset variable_name 

# 注意 ${} 是打印用的
echo "${varaible_name}"
```

command execution mode
- Batch mode
- concurrent mode

```shell
# batch mode command1执行完成后, 才会执行 command2
command1; command2

# concurrent mode 两条命令一起执行
command1 & command2

# 一条命令执行的同时， 我可以继续使用shell来执行其他的命令
command1 &
```

判断
```shell
# test 命令
[ -e /xxx/ ] # 判断文件是否存在, 存在返回 0, 在脚本中更常用
[ -d /dir ] # 判断目录是否存在
[ ! -e /xxx/ ] # 取反
[ -a /xxx/ ] # 
[ -r /xxx/ ] # 看当前用户是否有权限

[ string1 = string2 ] # 比较两个字符串, 必须带有空格
[ $score -ge 80 ] # 大于等于

# 增强版, 支持正则表达式
[[ $file =~ .*\.conf$ ]] # =~ 支持右侧写扩展的正则表达式, 判断文件后缀
# == 支持通配符
[[ $file == *.txt ]] 
# 判断是否为空
[ "$name" ] # 必须要加双引号, 不然无法判断带有空格的字符串

(cmd) # 会创建一个子进程 
{ cmd; cmd }

test /xxx/ # 存在返回 0, 不常用
```
# String
```shell
# 命令返回值进行变量赋值
today=$(date +%Y%m%d)
# 字符串的拼接
weather_report=raw_data_$today
```
用户输入
```shell
# 提示用户输入
read -p "Please input your name: " name
# 同时对多个变量进行赋值
read x y z <<< "xxx yyy zzz"
```
# 条件语句
```shell
# -a 表示 and
[ $ID = "ubuntu" -a $VERSION_ID = "20.04" ]
# -o 表示 or
[ $ID = "ubuntu" -o $ID = "rocky" ]
# 使用正则表达式
[[ $ID =~ rocky|cenos|rhel ]]

# 短路与


```

```shell
# the number of the args read by command line
if [[ $# == 2 ]] ; then
  echo "number of arguments is equal to 2"
else
  echo "number of arguments is not equal to 2"
fi

if [ "$string_var" == "Yes" ] ; then 
	echo "the string is equal"
elif [ "$string_var" == "No" ] ; then
	echo "the string is not equal"
else
	echo "no response is correct"
fi
```
and
```shell
if [ condition1 ] && [ condition2 ]
then
    echo "conditions 1 and 2 are both true"
else
    echo "one or both conditions are false"
fi
```
case
```shell
case $n in 
1|3|5) # 通配符
	cmd
	;; # 语法要求必须要有
2|4|6)
	cmd
	;;
*)     # 通配符
	cmd
esac
```
or
```shell
if [ condition1 ] || [ condition2 ]
then
    echo "conditions 1 or 2 are true"
else
    echo "both conditions are false"
fi
```

```shell
# 使用多个字符串
for i in a b c; do # 序列可以是回车, 空格, tab
	echo i=$i
done

# 使用序列
for i in {1..10}: do
	echo i=$i
done

# 使用通配符
for i in /data/scripts/*.sh; do
	echo i=$i
done 

# 使用命令
for i in {1..9}; do 
	for j in `seq $i`; do
		echo -en "${j}x${i}=$[i&j]\t" # 因为 \t是特殊符号, 所以要用-e
	done
	echo
done

break # 提前退出循环

# c 语言风格写法
for (( i=0; i<=6; i++ )) ; do
  echo $i
done

```

logical operators
```shell
$a == 2
a != 2
a <= 3
a -le 3 # 小于等于
a -lt 3 # 小于
a -ge 3 # 大于等于
a -gt 3 # 大于
a -eq 0 # 等于 0
```

# arrays
```shell
my_array=(1 2 "three" "four" 5)
declare -a empty_array
# add element to array
my_array+=("six")
my_array+=(7)

# print the first item of the array:
echo "${my_array[0]}" 
# print the third item of the array:
echo ${my_array[2]}
# print all array elements:
echo ${my_array[@]}

```

# Loop
```shell
for item in ${my_array[@]}; do
  echo $item
done


for i in ${!my_array[@]}; do
  echo ${my_array[$i]} # i is index
done



# 0, 1, 2, 3, 4, 5
for i in {0..5}; do
    echo "this is iteration number $i"
done

# 文件列表
for file in $(ls);do

done
```

for loop example
```shell
#!/usr/bin/env bash
# initialize array, count, and sum
my_array=(1 2 3)
count=0
sum=0
for i in ${!my_array[@]}; do
  # print the ith array element
  echo ${my_array[$i]}
  # increment the count by one
  count=$(($count+1))
  # add the current value of the array to the sum
  sum=$(($sum+${my_array[$i]}))
done
echo $count
echo $sum
```

cut 的列变成数列
```shell
col0=($(cut -d "," -f1 $csv_file))
```


# 脚本案例
```shell
#!/bin/bash
# 打印 CPU 型号
echo -e "CPU: \c"
lscpu | sed -nr "s#^Model name: +(.*)#\1#p"
head -n1 /proc/meminfo | tr -s " "
echo -e "/dev/sda: \c" 
lsblk /dev/sda | grep "^sda" | tr -s " " | cut -d" " -f4
echo -n "OS: " 
sed -rn 's/^VERSION="(.*)/\1/p' /etc/os-release
```
规避 rm 执行风险
```shell
WARNING_COLOR="echo -e \E[1;31m"
END="\E[0m"
DIR=/tmp/`date +%F_%H-%M-%S`
mkdir $DIR
mv $* $DIR
${WARNING_COLOR}Move $* to $DIR $END


# 配置脚本
chmod +x rm.sh
alias rm="/data/scripts/rm.sh"
```

安全选项
```shell
set -u # 当发现调用没有定义的变量时会报错
set +u # 关闭报错
set -o # 查看所有安全指令
set -e # 只要脚本出错就不执行
# 结合起来
set -u -e


rm -rf $Dir/* # 当 Dir 变量不存在时, 会直接删除根目录
```
格式化输出 printf
```shell
printf "%10s" 1 2 3 4 # 字符串输出, 预留 10 个字符宽度, 默认是右对齐
printf "%f\n" 1 2 3 4 # 打印出 4 行
printf "%s %s\n" 1 2 3 4 5 6 # 每两个数字换行
printf "%-10s" 10 # 左对齐
```
用 case 写菜单
```shell
echo -en "\E[$RANDOM%7+31];1m"
cat <<EOF
请选择:
1) 备份数据库
2) 清理日志
3) 软件升级
4) 软件回滚
5) 删库跑路
EOF
echo -en '\E[0m'
read -p "请输入上面的数字 1-5: " MENU
case $MENU in 
1)
	echo "执行备份数据库"
	;;
2)
	echo "执行清理日志"
	;;
3)
	echo "执行软件升级"
	;;
4)
	echo "执行软件回滚"
	;;
5)
	echo "执行删库跑路"
	;;
*)
	echo "输入错误"
	;;
esac
```