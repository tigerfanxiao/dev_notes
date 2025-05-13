
# system
```powershell
# 查看系统硬件和软件信息
\systeminfo.exe
```
### windows wifi password
```shell
# show all wifi profile stored on the computer
netsh wlan show profile
# show all wifi password 
netsh wlan show profile name="<ssid>" key=clear # clear for clear text
netsh wlan show profile name="<ssid>" key=clear | findstr Key # 只显示密码
```

### windows list all port 
```powershell
netstat -anob
# check if port 5000 is in use
netstat -anob | findstr 5000
```