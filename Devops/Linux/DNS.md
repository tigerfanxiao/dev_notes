
Bind9 是常见的 DNS 工具

配置方法

```shell
# 安装bind9
sudo apt update 
sudo apt upgrade -y
sudo apt install bind9 dnsutils -y



# 如果防火墙打开的话
sudo ufw status
sudo ufw allow bind9
```

假设本地是 dns 服务器
```shell
# 首先修改
cd /etc/bind
sudo vim ./named.conf.options

```

修改的内容, 就是说如果我本地解析不了这个域名, 我就发给 8.8.8.8 来解析
```shell
# 修改的内容
forwards {
	8.8.8.8 
}
```

重启 bind9
```shell
sudo systemctl restart bind9
sudo systemctl status bind9
```

关闭系统自带的域名解析服务
```shell
# systemd-resolved 是系统自带的 dns 解析起, 查看系统自带的 dns 配置
sudo systemctl status systemd-resolved
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved # 重启不再启动
```

设置客户端的 dns 为刚才的服务器 IP 地址
```shell
dig linux.org # 查看 dns 解析的服务器是不是自己定义的服务器地址
# 在客户机上清空 dns 缓存
sudo rndc flush 
```

# 设置内网的域名解析

假设要在内网映射一个域名 gxms-local.cainiao.com, 需要创建两个文件, 修改一个文件
- `/etc/bind/db.gxms-local.cainiao.com` 用于域名到 IP 地址的映射
- `/etc/bind/db.172.16.25` 用于 IP 地址到域名的映射
- `named.conf.local `


文件`/etc/bind/db.gxms-local.cainiao.com` 
```shell
$TTL    604800
@       IN      SOA     ns1.gxms-local.cainiao.com. admin.gxms-local.cainiao.com. (
                              2024060801 ; Serial
                              604800     ; Refresh
                              86400      ; Retry
                              2419200    ; Expire
                              604800 )   ; Negative Cache TTL
;
@       IN      NS      ns1.gxms-local.cainiao.com.
@       IN      NS      ns2.gxms-local.cainiao.com.

ns1     IN      A       172.16.25.244
ns2     IN      A       172.16.25.243

@       IN      A       172.16.25.244
www     IN      A       172.16.25.244


ftp     IN      CNAME   www.gxms-local.cainiao.com.

```

其中 ns1表示主要的 dns 服务器, ns2 表示备份dns 服务器
admin.gxms-local.cainiao.com. 不能遗漏, 表示 dns 网络管理员的邮箱, 等于 `admin@gxms-local.cainiao.com`

文件 `db.172.16.25` 把 IP 地址 172.16.25.244 映射到 gxms-local.cainiao.com

```shell
;
; BIND reverse data file for 172.16.25.x subnet
;
$TTL    604800
@       IN      SOA     ns1.gxms-local.cainiao.com. admin.gxms-local.cainiao.com. (
                2024060803 ; Serial
                604800     ; Refresh
                86400      ; Retry
                2419200    ; Expire
                604800 )   ; Negative Cache TTL
;
@       IN      NS      ns1.gxms-local.cainiao.com.
@       IN      NS      ns2.gxms-local.cainiao.com.

244     IN      PTR     gxms-local.cainiao.com.
243     IN      PTR     ns2.gxms-local.cainiao.com.

```

最后 `named.conf.local `中引用这两个文件
```shell
zone "gxms-local.cainiao.com" {
        type master;
        file "/etc/bind/db.gxms-local.cainiao.com";
};

zone "25.16.172.in-addr.arpa" {
        type master;
        file "/etc/bind/db.172.16.25";
};

```

下面测试这个私有的域名是否配置成功
```shell

# 测试两个文件是否配置成功
sudo named-checkzone gxms-local.cainiao.com /etc/bind/db.gxms-local.cainiao.com
sudo named-checkzone 25.16.172.in-addr.arpa /etc/bind/db.172.16.25
sudo systemctl restart bind9

```

在客户机上测试

```shell
dig gxms-local.cainiao.com
dig -x 172.16.25.244

ping gxms-local.cainiao.com
ping www.gxms-local.cainiao.com

```