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

```shell
# set and queries runlevel settings for services
# 设置 httpd 服务在 run level 3 启动
chkconfig httpd --level 3 on  
chkconfig --list | less 
service httpd restart
```

```shell
cd /etc/systemd/system

```


custom service
```bash
[Unit] 
Description=Custom 
Service Wants=network-online.target 
After=network-online.target

[Service] 
Type=forking 
ExecStart=/usr/bin/emacs --daemon 
ExecStop=/usr/bin/emacsclient --eval "(kill-emacs)" 

[Install] 
WantedBy=default.target
```


```shell
systemctl start custom.service
systemctl status custom.service
```