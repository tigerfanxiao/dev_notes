# Linux Boot Process
1. BIOS 检查所有的硬件
2. Boot sector 在硬盘上找到引导启动操作系统的 sector
3. 由 boot sector 引导启动Linux Kernel
4. Initial RAM disk 会调用驱动程序, 文件系统
5. 操作系统层级的服务初始化 Initialisation system 即 Systemd

# Initialization system
1. 首先会运行 `/sbin/init`, 这个又会调用`/etc/rc.d/rc.sysinit`, 再调用`/etc/inittab` 

- redhat based:  `/etc/rc.d` 这个目录下是所有
- Debian based: `/etc/init.d`

在 CentOS6 以前, 系统的 run level. 所有的用户, 都会在同一个 run level 里面
0. Halt
1. Single user Mode
2. Multi-user Mode (no networking)
3. Multi-user mode (with networking)
4. unused
5. Multi-user, with networking and a graphical desktop
6. Reboot

CentOS7 以后就用 target

# 命令

### Centos7 之前
```shell
# set and queries runlevel settings for services
# 设置 httpd 服务在 run level 3 启动
chkconfig httpd --level 3 on  
chkconfig --list | less 
service httpd restart
```

# `systemctl`

```shell
# 查看默认的 target
systemctl get-default 
# multi-user.target # 正常多人访问的服务
# 如果是使用 gui 则反馈
# graphical.target 

# 把默认 target 设置为 graphical.target
sytemctl set-default graphical.target
 
# 查看所有的loaded target
systemctl list-unit --type target
#只跑 multi-user.target 及所依赖的 target, 其他的 target 都停止
systemctl isolate multi-user.target
# 启动服务 查看服务状态
systemctl start custom.service
systemctl status custom.service
```


# Custom Service

```bash
vim penguin.target

# 文件内容
[Unit] 
Description=Penguin Target
Requires=multi-user.target # 依赖于 multi-user.target 
Wants=pengguin.service
Conflicts=rescue.service rescue.target
After=multi-user.target rescue.service rescue.target # 先等 multi-user.target 运行完成后再运行
AllowIsolate=yes

[Service] 
Type=forking 
ExecStart=/usr/bin/emacs --daemon 
ExecStop=/usr/bin/emacsclient --eval "(kill-emacs)" 

[Install] 
WantedBy=default.target


```

```shell
# 系统定义的 target 都定义在这个目录中
cd /usr/lib/systemd/system
cat multi-user.target # 查看 target

# 自定义 Target 的
cd /etc/systemd/system
ls multi-user.target.wants


mkdir penguin.target.wants
cd penguin.target.wants
sudo ln -s /usr/lib/systemd/system/httpd.service .




```