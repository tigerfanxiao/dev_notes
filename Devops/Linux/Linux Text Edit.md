# CAT
- cat 可以用于合并两个文件. 只是内容上的叠加
```shell
# 可以看到文件中的空格和 tab 键, 回车
cat -A a.txt
# 显示空行的行号
cat -n a.txt
# 倒着显示内容
tac a.txt
# 同一行内的内容, 倒过来显示
rev a.txt
# 查看文件内容, 并显示行号, 但是空行不计算
nl a.txt
```
# More
- more 很少用, more 的问题是文件翻到底, 会自动退出
# Less
- less 是增强的版本的 more, 翻到底不会退出
# head
```shell
head -n 3 <filename> # 头三行 

head -n -3 <filename> # 除了最后 3 行外的所有行
```
# Tail
```shell
tail -n 3 <filename> # 倒数 3 行
# 从第二行以后都要显示
tail -n +2 <filename> 

# 跟踪一个日志
tail -f <filename>
```

# CUT
```shell
cut -d: -f1,3 /etc/passwd # 分隔符:, 取第一列和第三列
cut -d: -f1,3-5 /etc/passwd
```
# hexdump
- 查看非文本, 展示是 16 进制
```shell
hexdump -C <filename>
```
# Paste
- 合并两个文件, 把各自的 line1 的内容何在一个, 用 tab 分割

```shell
paste a.txt b.txt
# 把竖着的文件, 按照行来显示, 类似与矩阵转至, 用 tab 分割
paste -s a.txt 
# 用空格作为分隔符
paste -s -d" " a.txt
```

# wl
- 单词统计
```shell
wc -l a.txt # 统计一个文件的行数, 会同时返回文件名
cat a.txt | wc -l # 统计一个文件的行数, 不返回文件名
```
# sort
- 默认对文件内容的排序是按照文件的每一行的第一个字符开始排序
```shell
# 对文件的中间某一列进行排序
sort -t: -k3 /etc/passwd # -t: 表示:是分隔符,  第三列
sort -t: -k3 -n /etc/passwd # 按照数字的大小排序, 而不是字符的顺序排序
sort -t: -k3 -nr /etc/paswd # 倒叙排序

df | tail -n + 2| tr -s ' ' % | cut -d "%" -f5 | sort -nr


# 合并文件并去重, 然后输出
cat a.txt b.txt | sort -u > c.txt
```

# uniq
- 只对连续出现的内容去重
```shell
uniq -c a.txt # 去重, 并计算每个元素出现的次数
uniq -u a.txt # 去重, 留下哪些只出现过一次的
uniq -d a.txt # 去重, 留下那些出现过多次的

cut -d' ' -f1 access_log | sort | uniq -c | sort -nr | head -n3

# 取出两个文件的相同行
cat a.txt b.txt | sort | uniq -u
# 取出两个文件的不同行
cat a.txt b.txt | sort | uniq -d
```
# Diff
```shell
diff a.txt b.txt

diff -u a.txt b.txt > diff.log # 怎么让 a 文件变成 b文件的方法放在 log 中
patch -b a.txt diff.log # 按照 diff.log 恢复出来, 把 b 文件恢复出来, 保存在a.txt中, 并把原来的 a文件做了备份
```
# vimdiff
- 并列两个窗口看文件
```shell
vimdiff a.txt b.txt
qall # 全部退出
```
# cmp
- 比较两个 2 进制文件
```shell
cmp /bin/ls /bin/dir
/bin/ls /bin/dir differ: byte 793, line 1
hexdump -s 790 -Cn20 /bin/ls # 跳过前面 800 个字符, 看后面 20 个
hexdump -s 790 -Cn20 /bin/dir
```
# grep
- 文本处理三剑客 grep, sed, awk
- `grep <word> stdin` 如果没有带文件, 那就是用来接收标准输入的
- 多用于处理标准输出, 而不是文件
``` shell
# 使用管道符
ifconfig eth0 | grep netmask

# 使用标准输入重定向
grep root < /etc/passwd

# 只看前三个结果
grep -m3 nologin /etc/passwd

# 取反或者过滤
grep -v nologin /etc/passwd
grep -v "#" /etc/fstab # 过滤掉注释行

# 不区分大小写
grep -i ROOT /etc/passwd

# 统计出现的次数
grep -c root /etc/passwd

# 只显示匹配到的字符串
grep -o root /etc/passwd

# quiet mode, 与 $? 连用, 判断上一个命令是否有返回
grep -q root /etc/passwd
if [ $? -eq 0 ]; then
    echo "Command succeeded"
else
    echo "Command failed"
fi

# 后续行数
grep -A3 root /etc/passwd # 在 root 后面的 3 行, After
# 前多少行
grep -nB3 root /etc/passwd # Before 并列出行号
# 前后多少行
grep -C3 root /etc/passwd 

# 包含多个关键词, 或者关系
grep -e root -e bash /etc/passwd
# 包含多个关键词, 并且, 使用两个命令的组合
grep root /etc/passwd | grep bash

# 如果把过滤条件放入文件(每一行为一个条件, 条件之间是或的关系), 使用这个文件过滤另一个文件. 
grep -f test.txt /etc/passwd 
# 快速找出两个文件的相同之处
grep -f a.txt b.txt 

# 递归查询文件内容, 但不处理软连接
grep -r root /etc/* # 查询所有文件中的内容
# # 递归查询文件内容, 处理软连接
grep -R root /etc/*


# 使用命令的结果来搜索
grep `whoami` /etc/passwd
grep "$USER" /etc/passwd

# 扩展的正则表达式
grep -E 
egrep # 扩展的正则表达式

```

# 正则表达式
- 通配符和正则的区别
	- 通配符多用来匹配文件名中的字符串
	- 正则用来表示文本中的字符串
- 正则表达式区分为, 基本的正则表达式, 扩展的正则表达式, perl 语言的正则表达式
- 通配符中的`[a-z]`和正则中的`[a-z]` 不同. 通配符的字母排序是`aAbB...`, 正则中的排序是`ab..AB..` 
- 通配符中的`*`表示任意字符, 正则中的`*`表示前面的符号出现任意次数
- 正则匹配模式是贪婪模式, 竟可能匹配更多的
```shell
. # 字符串中的一个字符, 包含空格, 符号

ls -al | grep "f[[:lower:]]" 

[.] # 写在中括号中的点只表示. 不是任意字符
* # *前面的符号出现任意次数, 包含 0 次

? # a可有可无, 出现零次或者 1 次
+ # 出现一次以上
{n} # 出现 n 次
{,5} # 出现 0 到 5 次
{5,} # 出现 5 次以上

^$ # 表示空行
# 排除注释和空行
grep -v "^#" -e "^$" /etc/apache2/apache2.conf
grep -v "^[#|^]$" /etc/apache2/apache2.conf
\<word\> # 单词, 数字, 字母, 下划线都属于单词
\bword\b # 可以表示单词的词首, 词尾

# 词组
(abc) # 分组可以用 \1 来也引用
ip 地址
([0-9]{1,3}\.){3}[0-9]{1,3}


```
# sed
- grep 不能修改文件, vim 是一种交互式的操作. 如果要非交互的, 批量修改文件则用 sed
- sed是行编辑器, 他不会像 vim 一样把整个文件都加载到内存中, 而是每次只处理一行, 第一行处理完就退出内存. 从上往下处理
- sed 每次处理一行, 默认就会打印一行. 适合处理大容量的文件
- 和 grep 一样, 在没有文件时, 支持标准输入, 每次一行
- `//`里面是正则表达式, 多个正则表达式用逗号分开
- sed

```shell
sed 'p' /etc/fstab # 打印当前模式空间的行. 读一行打印一次, 再打印一次
sed '10p' /etc/fstab # 除了自动打印外, 再打印第 10 行
sed -n '10p' /etc/fstab # 关闭自动打印

sed -n "/network/p" # 查找 network, 并打印
sed -n '3,5p' # 打印第 3 行到第五行

sed -n '/^b/,/^s/p' /etc/passwd # 从 b 开始的行, 找到 s 开始的行, 注意, 匹配中 b 之后 即使找不到 s, 也会打印出来

# 打印 12 点到13 点的日志
sed -n '/^Mar 30 12:/,/^Mar 30 14:/p' test.log
# 打印 12 点或17 点的日志
sed -n -e '/^Mar 30 12:/p' -e '/^Mar 30 17:/p' test.log
# 或者用; 分割两个脚本
sed -n -e '/^Mar 30 12:/p;/^Mar 30 17:/p' test.log
# 打印奇数行
sed -n '1~2p' 

# 不显示第一行, 但是后面自动打印
sed '1d'
sed '1~2d' # 把奇数行不显示了, 只打印偶数行

# 直接修改文件本身, 使用.bak 做备份
sed -i.bak '/^#d/' fstab # 删除文件中的注释


# 追加
seq 10 | sed '5a hello' # 在第五行的后面追加 hello, 并自动忽略空格
seq 10 | sed '5a\   hello' # 在第五行的后面追加 hello, 不自动忽略空格
sed -i '/^abcd123$/a ABCD123' test.txt # 追加 ABCD123 到 abcd123
# 替换
seq 10 | sed '5c hello' 
# 替换
sed -i.bak '/^SELINUX=enforcing/c SELINUX=disable' /etc/sysconfig/selinux
# 读文件
sed '1~2r /etc/issue' # 在奇数行的后面, 读入 /etc/issue文件的内容
# 取反
seq 10 | sed -n '1~2!p'  # 打印出偶数

# 后项引用
echo abc123xyz | sed -r 's/(abc)(123)/\1new/' # \1 引用第一个(abc)
echo abc123xyz | sed -r 's/(abc)(123)(xyz)/\3/' # 删除第一, 第二个分组, 只保留第三个分组
# &表示搜索到的内容的引用
echo abc123xyz | sed -r 's/.*/&new/' # 把搜索到的内容加上 666
sed -r 's/^(SELINUX=).*/\1disabled/' /etc/sysconfig/selinux # 关闭 selinux
# 查看 IP 地址
ifconfig eth0 | sed -rn '2s/^.*inet ([0-9.]+).*/\1/p'


sed -ir.bak 's/^(GRUB_CMDLINE_LINUX=.*)"$/\1 net.ifnames=0/'

sed -i '/^(GRUB_CMDLINE_LINUX=/s#"$# net.ifnames=0#' /etc/default/grub

# 搜索内容并替换构造新的文件
sed 's/6379/6380/g' test.conf > test2.conf # g表示全局替代

# 动态替换内容
sed "s/6379/`id -u wang`/g" test.conf
sed "s/6379/$UID/g" test.conf
sed 's/6379/'$UID'/g' test.conf

df | sed -rn 's/^\/dev\/sd.* ([0-9]+%).*/\1/p' 
```
保持空间
- 把读过的行, 暂时存起来.
- 我们上面看到的模式空间
- 两个空间和相互追加, 覆盖
```shell
sed -n 'n;p' File # n 读取匹配到的行的下一行覆盖只模式空间. 打印偶数行
seq 10 | sed 'N;s/\n//' # N 读取匹配到的行的下一行追加到模式空间
```


# awk
- 使用 awk 打印列

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



# Use Case

```javascript
export default defineComponent({
// TODO: 修改 Helloworld ciw
	name: 'HelloWorld'
	props: {
		msg: String,
	},
	setup() {
		// TODO: 修改泛型 ci<
		// TODO: 删除泛型 di< da<
		const count = ref<number>(0)
	}
	return {
		// TODO: 删除所有返回值 di{
		add, 
		bb, 
		cc
	}
})
```


# Motion 动作
i = inner 
a= around
选中双引号中的所有字符 i"
选中双引号中的所有字符, 包括双引号本身 a"

选中光标所在的单词 iw
选中光标所在单词, 包含前后的空格 aw
选中小括号内的字符 i( 或者 ib
选中小括号内的字符包含括号本身 a(
选中花括号内的字符 i{ 或者 iB
选中 html tag 中的字符 it
is 句子
ip 段落

# Vim
```shell
G # 文末
gg # 文首
```
## Visual 模式

```shell
v # 进入可视化模式, 选中单个字符
V # 进入可视化模式, 选中整行

ctrl + v # 纵向光标

viw # 选中一个单词
vib # 选中括号中的内容

# 可视化模式下的操作
d # 删除选中内容
```

案例: 在选中行的行首快速插入#
```shell
20G # 光标移动到 20 行
ctrl + v
# 使用方向键选中多行
I # 进入插入模式
# # 输入井号键
ESS # 退出插入模式

# 此时#号被插入多行
```
案例: 删除多行注释
```shell
20G # 光标移动到 20 行
ctrl + v
# 使用方向键选中多行
d # 直接删除
```
### 分屏
```shell
ctrl + w + v # 左右分屏
ctrl + w + s # 上线分屏
ctrl + w # 在两个分屏之间切换

ctrl + w + q # 删除当前窗口
ctrl +w + o # 删除所有窗口
```

## 扩展命令模式
```shell
:r <filename> # 将文件内容读到当前文件中
:w <filename> # 另存为
!command # 执行命令
r!command # 读入命令的输出
```

删除
```shell
:2d # 删除第二行
:2,5d # 删除第二行到第五行
:.,$d # 删除当前行到结尾
```

复制
```shell
:2,4y # 复制2到 4 行
p # 粘贴到光标的行
P # 粘贴到光标的前一行

:30:r /etc/isse # 在第 30 行读入一个新文件的内容
```
搜索替代
```shell
:%s/root/ROOT/ # 同一行只替代第一个
:%s/root/ROOT/g # 全局替换

# 在每一行的行首加#
:$s/^/#/
```
加行号
```shell
:set nu # 显示行号
:set number
:set nonu # 不显示行号
```
通过修改 vim 的配置文件加行号
```shell
# /etc/vimrc 全局修改
# ~/.vimrc # 个人的配置

# 在文件中加入
set nu
```
忽略大小写
```shell
:set ic # igonrecase
:set noic
```
缩进
```shell
:set ai # autoindent 自动缩进 默认是 8 个
:set noai

:set shiftwidth=4 # 配置缩进为 4 个字符
>> 缩进
<< 反缩进
```

保留格式
```shell
:set paste
```
显示不可见字符
```shell
:set list # tab键是 ^I
```
高亮
```shell
:set hlsearch 
:set nohl # 取消高亮
```
语法高亮
```shell
:set syntax
```
把 windows 文件转换为 linux 文件
- vim会把 windows 格式识别为 dos 文件
- 使用 `hexdump -C win.txt` 可以看到 `0d 0a`, 如果是 linux 文件不会看到 `0d`
```shell
:set ff=unix # 转换为 linux 格式
:set ff=dos # 转换为 windows 格式
```
tab 用空格替代
```shell
:set et # extend tab 把 tab 转换为空格, 默认是 8 个空格
:set ts=4 # 一个 tab 键=4 个空格
```
加横线标记
```shell
:set cul # cursorline
```
加密
```shell
:set key=<yourpass>
:set key= # 清空密码
```