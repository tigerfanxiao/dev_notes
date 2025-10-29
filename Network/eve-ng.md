
EVE-NG 承载在 ubuntu 的上
登录的目的是增加配置镜像
启动 eve-ng 镜像后, 可以看到 eve-ng 所运行的 IP 地址

```shell
# ubuntu 管理员账户密码
root/eve

# web页面登陆密码
admin/eve

# 查询 eve 运行的本地 ip 地址
ip add | grep 192.
```

### Telnet 
- 不需要用户名密码
- 用 SecureCRT 登录, 端口号从 32769 开始

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

### vmdk
vmdk 文件应该是需要转化成qcow2文件. 转化的时候, 可能需要配置 yml 文件

```shell
# 转化 vmdk 文件到 qcow2 文件的命令
/opt/qemu/bin/qemu-img convert -f vmdk -O qcowe <vmdk> virtio.qcow2

# 这里可能是放设备图标的地方
/opt/unetlab/html/images/icons 
```

# wireshark 抓包问题
用 putty 登录 eve 环境
然后就能抓包了
