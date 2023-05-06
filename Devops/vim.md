# 模式
vim 有下面 4 中模式
|模式|作用|转换|注释|
|---|---|---|---|
|Normal|移动光标|`i` 转 Insert 模式||
|Insert|键盘输入|`jj` 或者 `esc`退出 Insert 到 Normal||
|Vistual|选择内容|`v` 进入或者退出 Visual 模式| ctrl + v block select|
|Command|查找, 替换, 保存|||


## 模式转换
### Normal -> Insert
在光标前插入 i
在行首插入 I

在光标后插入 a
在行尾插入 A



### Insert -> Normal 
esc
jj 在配置文件中做的


### Normal -> Visual
v
### Visual -> Normal
esc
v


### Normal -> 命令模式
:
### 命令模式 -> Normal
esc
保存和退出文件 ZZ 或者 `:wq`

# 光标移动

### 文档范围的移动(Normal)

文档头部 gg
文档底部 G
跳转到第 12 行 12G
移动到文档 50%的地方 50%

### 命令模式下
跳转到第 12 行 12

### 行内移动
跳到到行首 0
跳到行首第一个非空字符 ^
跳到行尾 $

光标跳到下一个{char}所在的位置 f{char}
反向移动到上一个 {char} 所在的位置 F{char}
光标跳到下一个{char}的前一个字符的位置 t{char}
光标反向移动到上一个{char}的后一个字符的位置  T{char}
重复上一次的字符查找操作 ;
反向查找上一次的查找命令 ,


### 单词为单位(Normal)
下一个单词开头 w
本单词或上一个单词开头 b
本单词或下一个单词词尾 e
上一个单词结尾 ge

### 字母范围移动
向左 h
向下 j
向上 k
向右 l



# 修改内容

复制 y + p

在下一行插入 o
在上一行插入 O
向下插入 5 行, 并同时编辑 5 行 5o

### 删除
删除一个字母 x
删除一行  dd
删除下面的 5 行 5dd


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

diw 删除一个单词

# 操作符 Operator
操作符是配合Motion 做的, 如果按一个 d, 会等待下一步操作
d delete 删除
c Change 修改
y yank 复制
v visual 选中并进入 visual 模式
u 撤销
ctrl + r 重做

dd 是删除一行
cc 是删除当前行, 并进入写入模式
yy 复制一行 p 粘贴

删除到行末 d$
向下删除 2 行 3dd
删除 2 行进行写入模式 2cc
删除整个文档 cie  这里 e 表示 entire

删除 div内所有的内容(包含内签的 div) dit 

修改当前字符到下一个 s 位置的内容 cfs

# Visual 模式
按 v 进入 visual 模式之后, 
viw 选中一个单词
vib 选中括号中的内容


# 大小写转换
### Normal
将光标下的字母改变大小写  ~
将光标位置开始的 3 个字母改变大小写 3~
改变当前行字母的大小写 g~~
将当前行字母改成大写 gUU
将当前行字母改成小写 guu
将光标下的单词改成大写 gUaw (gUiw)
将光标下的单词改成小写 guaw(guiw)
# Visual
viw 选中单词 + U 变成大写


# Tips
查看函数的实现 
cmd + 鼠标左键 = g + d = go to definition
ctrl +  -

显示函数签名
g + h  = go hover

往后跳标签页
g + t
往前跳
g + T
4gt 跳到第 4 个标签页

在命令模式下
:tabn 下一个标签页
:tabp 上一个标签页
在 settings.json中做配置
```json
"vim.normalModeKeyBindingsNonRecursive": [
	{
		"before": ["t", "h"],
		"commands": [":tabp"]
	},
	{
		"before": ["t", "l"],
		"commands": [":tabn"]
	}
]

```

光标从编辑器跳到侧边栏
cmd + 0 然后用 j k 上下选中文件
空格 可以展开或关闭文件夹
空格 打开文件
l 或者 cmd + 1 光标回到编辑器

将当前屏分割到右边 cmd + \
在分屏模式下
cmd + 2 跳到右面的屏幕
cmd + 1 跳到第一个屏幕


# vim easymotion 
看文档
默认已经开启了 `<leader> `模式设置为 space
```html
<leader><leader> s <char> 搜索一个字符
<leader><leader> w <char> 搜索一个字符
<leader><leader> e <char> 搜索一个字符
```

```json

 "vim.normalModeKeyBindingsNonRecursive": [
        {
            "before": ["<leader>", "t"],
            "commands": ["workbench.action.terminal.focus"]
        },
 ]   
```
surround command
删除字符两边成对出现的
cs"'
ysiw' 给字符两边加'

# 多光标模式 
cmd + d 选中的相同的单词 + c 修改
在 vim 中用gb实现相同的操作 + c 修改
I 在前面删除

# 将光标定位到 terminal

cmd + shift + p -> focus termianl  或者用 空格 + t
cmd  + 1 回到编辑器
在设置中 <leader> 被定义为空格
<leader><leader> s <char> 搜索一个字符


vim中的查找 /<char>

新增 4 个空行
复制一个空行 yy 粘贴 4 次 4p

把下面多行, 移动到当前行的末尾 J


# 保存文件

ctrl + g 查看当前 buffer
:list 查看所有 buffer
