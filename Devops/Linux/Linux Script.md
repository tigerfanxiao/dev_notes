
# shebang
在 linux 中运行的 python 脚本需要用
```shell
#!/usr/bin/env python3
# -*- coding=utf-8 -*-
```

在 linux 中运行的 shell 脚本


```shell
#! /bin/bash
```

执行脚本
```shell
chmod u+x helloworld.sh
./helloworld.sh

# 如果文件没有执行权限
bash helloworld.sh
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


案例
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


符号
```shell
# 注释
; # 如果在同一行放两个命令, 则用;隔开
* # 通配符
? # 单个字符
\ # escape
touch file\ with\ space.txt # 有空格的文件
"" # 里面的变量会被引用
'' # 里面的变量不会被引用
> # 覆盖, 重新构建
>> # 增加在文件后面
2> # redirect standard error to file
2>> # append error message to file

< # redirect file content to standard input
sort < data.txt
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

```

command execution mode
- Batch mode
- concurrent mode

```shell
# batch mode command1执行完成后, 才会执行 command2
command1; command2

# concurrent mode
command1 & command2
```

condition
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

arrays
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

for loop
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