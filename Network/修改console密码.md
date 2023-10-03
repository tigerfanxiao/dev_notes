
1. 如果交换机已经上电, 不需要对交换机下电
2. ctrl + B 进入 bootrom
3. 默认bootrom密码是 Admin@huawei.com 
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
  
Clear password for console user successfully. Choose "1" to boot, then set a new  password  
Note: Do not choose "Reboot" or power off the device, otherwise this operation will not take effect
```

在系统中配置console密码

```shell
sys
user-interface console 0
authentication-mode password cipher Huawei@123
```

不设置console密码
```shell
system-view
user-interface console 0
authentication-mode none
quit
save
```