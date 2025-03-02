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
# VIM
```shell
vimtutor
```
# 模式
- vim 有下面 4种模式

| 模式      | 作用         | 转换                              | 注释                    |
| ------- | ---------- | ------------------------------- | --------------------- |
| Normal  | 移动光标       | `i` 转 Insert 模式                 |                       |
| Insert  | 键盘输入       | `jj` 或者 `esc`退出 Insert 到 Normal |                       |
| Vistual | 选择内容       | `v` 进入或者退出 Visual 模式            | ctrl + v block select |
| Command | 查找, 替换, 保存 | :                               |                       |


### 模式转换
- Normal -> Insert
```shell
i # 在光标前插入 
I # 在行首插入 
a # 在光标后插入 
A # 在行尾插入 
o # 在新一行插入
O # 在光标的上一行插入
```

## VIM 命令模式
保存和退出
```shell
ZZ # 存盘退出
ZQ # 不存盘退出
```
光标行间移动
```shell
gg # 文档头部 
G # 文档底部 
12G # 跳转到第 12 行 

H # 当前页的第一行
M # 当前页的中间
L # 当前页的底部

w # 下一个单词的词首
e # 当前或者下一个单词的词尾
b # 当前或者下一个单词的词首
5w # 下面 5 个单词
```
光标行内移动
```shell
h # 左移
l # 右移
j # 下移
k # 上移
^ # 非空白字符的行首
0 # 包含空白字符的行首
$ # 行尾
```
段落移动
```shell
} # 下一段
{ # 上一段
```
复制行
```shell
yy # 复制光标当前行
3yy # 复制光标当前所在的连续三行

100iwang+esc # 把wang 复制了 100 次
```
删除替换
```shell
# 删除字符
x # 剪切一个字符
r # 替换一个字符
R # 进入 replace 模式

# 删除单词
diw # 删除一个单词, 光标在单词的任意位置
dw # 删除光标所在位置到单词结尾

# 删除行
dd # 删除一行
3dd # 删除
d0 # 删除到行首
d$ # 删除到行末
dgg # 删除到文件开头
dG # 删除到文件末尾
```
大小写
```shell
~ # 大小写切换
```
快速删除换行符
```shell
J # 不需要把光标切换到行末, 直接把下面一行提上来
```
撤销命令
```shell
u # 撤销 1 个动作
10u # 撤销 10 个动作
ctrl + r # 重新执行撤销的命令
. # 重复执行上一个操作
. # 重复执行上一个操作 9 次

```
搜索
```shell
/ # 搜索 n 下一个 N 上一个
```
高级用法
```shell
# 删除双引号中间的内容
di"
# 删除中括号中间的内容
di[ 
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




# VIM Visual 模式

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