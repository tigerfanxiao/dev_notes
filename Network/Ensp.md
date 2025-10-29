# 虚拟设备

一般实验, 使用 AR2220 系列就行了, 不要用 router



# 连接设备方式 SecureCRT
推荐使用 SecureCRT, 好用, 无需额外配置. 
Telnet 的地址是 127.0.0.1:2000



### 临时修改背景
1. option -> sssion -> appearance 修改背景是 traditional
### 永久修改背景
1. option -> global - general -> default-session > Edit default setting -> Emulation ->Termianl: Linux/Xterm-> check ANSI
2. Appearance -> traditional



# MobaxTerm

如果是 AR 的 com 端口号是 2000 , 则
回车出现`^M` 问题
1. 窗口右键 --> Change terminal setting 
2. Line Disipline Option : Force off
3. Local line Editing : Force off

参考 [blog](https://www.bilibili.com/read/cv15658602)
