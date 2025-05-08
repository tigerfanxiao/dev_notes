
# system
```powershell
# 查看系统硬件和软件信息
\systeminfo.exe
```
### wifi
```shell
# show all wifi profile stored on the computer
netsh wlan show profile
# show all wifi password 
netsh wlan show profile name="<ssid>" key=clear # clear for clear text
netsh wlan show profile name="<ssid>" key=clear | findstr Key # 只显示密码
```
