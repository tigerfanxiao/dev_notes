# Install openssh
```shell
sudo yum -y install openssh-server openssh-clients
sudo systemctl start ssh
```




使用公钥文件登录ssh
```bash
chown 400 perm
ssh usernanme@ip_addriess -i perm
```