### check linux version
```shell
lsb_release -a
```
### Switch root user
```shell
# switch to root without password
sudo su 
exit # exit root 
sudo -i  # switch to root with password of current user
```
# Package Management

```shell
apt-get update

apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/list*
```

