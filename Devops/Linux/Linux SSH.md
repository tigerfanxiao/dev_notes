# Install openssh
在服务器侧安装服务端
```shell
sudo yum -y install openssh-server openssh-clients
sudo systemctl start sshd
```

远程登录服务器
注意: 一般情况下 root 账户不能远程登录, 需要用别的账户
```shell
ssh username@<ip>
```


使用公钥文件登录ssh
```bash
chown 400 perm
ssh usernanme@ip_addriess -i perm
```