### 破解 root 密码
1. 重启服务器, 按 `e` 进入内核模式
![[Pasted image 20250131073810.png]]
1. 找到 Linux16所在的行. 把`ro` 改为`rw` 在行尾添加 `init=/bin/sh`
![[Pasted image 20250131073822.png]]


# 配置repo


Since CentOS 7 has reached EOL, the mirror is moved to vault. When yum is executed, it will return an error "Cannot find a valid baseurl for repo: base/7/x86_64".

We need to update `CentOS-Base.repo` in `/etc/yum.repos.d/CentOS-Base.repo`.

2. Uncomment the lines starting with `baseurl`.
3. Update all `http://mirrorlist.centos.org` to `http://vault.centos.org`.
4. Update all `http://mirror.centos.org` to `http://vault.centos.org`.    
5. We can clear the cache with `sudo yum clean all`.

```
repo_file=/etc/yum.repos.d/CentOS-Base.repo
cp ${repo_file} ~/CentOS-Base.repo.backup
sudo sed -i s/#baseurl/baseurl/ ${repo_file}
sudo sed -i s/mirrorlist.centos.org/vault.centos.org/ ${repo_file}
sudo sed -i s/mirror.centos.org/vault.centos.org/ ${repo_file}
sudo yum clean all
```

```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
mirrorlist=http://vault.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://vault.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates 
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://vault.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
baseurl=http://vault.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://vault.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
baseurl=http://vault.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
mirrorlist=http://vault.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
baseurl=http://vault.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```