
ubuntu 的登录 root/eve
登录的目的是增加配置镜像
在登录页面可以看到 eve 的 ip 地址 192.168.247.128

eve 页面的登录
admin/eve


# 添加镜像

镜像多种, 每一种添加方式不同

### .image 结尾的镜像
直接复制到 `/opt/unetlab/addons/dynamips/` 路径下, 就可以使用

.bin 文件
复制到 `/opt/unetlab/addons/iol/bin`
在 ubuntu 中给与权限
```shell
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

### .qcow2文件
在 `/opt/unetlab/addons/qemu/`下创建已镜像名的文件夹
```shell

mkdir /opt/unetlab/addons/qemu/<image_name>
# 复制.qcow2镜像到文件夹下
```

还需要做什么, 需要查询