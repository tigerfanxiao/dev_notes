# 网络

VMware会构建一个虚拟交换机。 这个交换机连接了虚拟机和宿主机的物理网卡
VMware有三种网络类型, 对应VMWare在宿主机上创建的 3 张虚拟网卡

| 网卡模式        | 网卡      |
| ----------- | ------- |
| Bridge      | VMNet 0 |
| NAT         | VMNet 8 |
| Hostly-only | VMNet 1 |

### 桥接网络
bridge对应的VMNet 0 网络，构建一个虚拟机的网卡和物理机网卡在同一个网段。 这种模式下的虚拟机在网络上和宿主机一个层级。 接受物理网络DHCP服务器分配的IP地址。 这种网络模式下， 虚拟机可以用作服务器

需要打开虚拟网络编辑器. 配置 VMnet0网卡, 指定桥接的宿主机上的物理网卡. 如果宿主机使用无线上网, 则需要指定宿主机的无线网卡, 否则指向宿主机上的有线物理网卡
![[Pasted image 20250202113002.png]]

### NAT模式
NAT 模式对应的VMNet 8网卡 ，这个网卡作为NAT转化。 这种模式下的虚拟机可以看作是一个内网。 虚拟机对外是不可见的 ，也就是外界只能看到宿主机的流量。 这种网络模式下， 虚拟机可以作为客户端。 

### Host-only 模式
Host-only模式对应的是VMNet 1网卡，VMNet 1网卡给host-only虚机分配ip地址。 此类虚机只能互相访问， 或者访问宿主机。 但是不能访问外网。 
