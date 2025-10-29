
1. 如果交换机已经上电, 不需要对交换机下电。 重启设备
```shell
reboot 
reboot fast # 不保存配置， 直接重启
```
2. ctrl + B 进入 bootrom
3. 默认bootrom密码需要查看产品手册
	V200R003: huawei
	V200R005C00 and later versions: Admin@huawei
4. 清除console密码
```shell
            MAIN  MENU  
  
    1. Boot with default mode  
    2. Boot from Flash  
    3. Boot from CFCard  
    4. Enter serial submenu  
    5. Enter ethernet submenu  
    6. Modify Flash description area  
    7. Modify BootROM password  
    8. Clear password for console user  
    9. Reboot  
  
Enter your choice(1-9):8  # 选择8清除console密码
Note: Clear password for console user? Yes or No(Y/N): y      
# 必须要选 1 不要选 8
Clear password for console user successfully. Choose "1" to boot, then set a new  password  
Note: Do not choose "Reboot" or power off the device, otherwise this operation will not take effect
```

在系统中配置console密码

```shell
sys
user-interface console 0
authentication-mode password
set authentication password cipher Huawei@123
```

不设置console密码
```shell
system-view
user-interface console 0
authentication-mode none

quit
save
```

