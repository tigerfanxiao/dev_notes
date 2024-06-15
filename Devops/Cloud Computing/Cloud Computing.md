云的计算资源类型可以分为 4 种
- Shared 类似 VM, 公用一个物理机上, 有多个 VM
- Transient 类似 Spot VM
- Reserved 
- Dedicated 类似 Bare Metal 
# Virtual Machine
## Hypervisor Types
有两种 hypervisor
一种是用 type1, 类似 exsi, 是直接把 hypervisor 装在裸金属设备(Bare Metal)上的, 没有操作系统. 另一种是 type2, 类似 VMware workstation. 先装了操作系统, windows, 再装hypervisor


# Acronyan

HPC High Performance Computing

# Network type for VMS
VMware会构建一个虚拟交换机。 这个交换机连接了虚拟机和宿主机的物理网卡
VMware有三种网络类型, bridge, nat, 和 host-only

bridge对应的VMNet 0 网络，构建一个虚拟机的网卡和物理机网卡在同一个网段。 这种模式下的虚拟机在网络上和宿主机一个层级。 接受物理网络DHCP服务器分配的IP地址。 这种网络模式下， 虚拟机可以用作服务器

nat模式对应的VMNet 8网卡 ，这个网卡作为NAT转化。 这种模式下的虚拟机可以看作是一个内网。 虚拟机对外是不可见的 ，也就是外界只能看到宿主机的流量。 这种网络模式下， 虚拟机可以作为客户端。 

Host-only模式对应的是VMNet 1网卡，VMNet 1网卡给host-only虚机分配ip地址。 此类虚机只能互相访问， 或者访问宿主机。 但是不能访问外网。 

