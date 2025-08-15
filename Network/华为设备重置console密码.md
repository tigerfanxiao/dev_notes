
有三种方法
1. 如果能用telnet登陆, 可以在设备里改
2. Clear the console login password in BootROM and change the console port password 必须重启
3. Rename the current startup configuration file in BootROM, restart the device with no configuration, and change the console port password. 必须重启

如何在不中断业务的情况下, 完成重置密码
1. 有种方法是迁移现有路由器的业务到主路由器, 然后重启
BootROM menu 是类似bios的东西. 也是需要重启才能看到
