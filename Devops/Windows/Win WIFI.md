
```shell
# show all wifi profile stored on the computer
netsh wlan show profile
# show all wifi password 
netsh wlan show profile name="O.MG" key=clear # clear for clear text
netsh wlan show profile name="O.MG" key=clear | findstr Key # 只显示密码
```