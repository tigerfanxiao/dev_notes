# Shell 脚本格式
- 脚本在执行时, 如果中间有错误, 还是能够继续执行下去的
shebang
- 所有的解释性语言, 首行都要写 shebang
在 linux 中运行的 python 脚本需要用
```shell
#!/usr/bin/env python3
# -*- coding=utf-8 -*-
```

在 linux 中运行的 Bash shell 脚本
```shell
#!/bin/bash
```

为脚本增加可执行权限
```shell
# 加执行权限
chmod u+x helloworld.sh
./helloworld.sh

# 如果文件没有执行权限
bash helloworld.sh

# bash 接受标准输入
echo hostname | bash 
# 把脚本放在服务器上, 下载直接运行
curl <url.sh> 2>/dev/null | bash # 把标准输出传给了 bash

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
env # 查看所有的环境变量
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
```shell
# 有两种方法
$(command)
`command`

here=$(pwd) # 把命令的结果保存到变量中

# 在数学运算中需要用(())
sum=$(($n1+$n2))

# 注意 ${} 是打印用的
echo "${varaible_name}"
```

command line arguments
```shell
./myshell.sh arg1 arg2

# 在脚本中用
$1 
$2 
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

# String

```shell
# 命令返回值进行变量赋值
today=$(date +%Y%m%d)
# 字符串的拼接
weather_report=raw_data_$today
```

# Condition
```shell
# the number of the args read by command line
if [[ $# == 2 ]] # integer comparison need double square bracket
then
  echo "number of arguments is equal to 2"
else
  echo "number of arguments is not equal to 2"
fi

if [ "$string_var" == "Yes" ]
then 
	echo "the string is equal"
elif [ "$string_var" == "No" ]
then
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
for file in $(ls) # [TASK 9]
do
	if (( `date -r $file +%s` > $yesterdayTS ))

	then

	fi
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

N=6
for (( i=0; i<=$N; i++ )) ; do
  echo $i
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