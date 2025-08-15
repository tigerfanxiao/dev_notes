打开注册表  win键 + regedit

```shell
Go to HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ USBSTOR. In the USBSTOR section, open the Start ⇨ parameter in the “Value” field, set 4 and click “OK”.
```

这样会看不到插上去的USB， 如果要恢复，就把value改为3

