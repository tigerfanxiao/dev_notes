LVM Logical Volume Management 虚拟卷管理


```shell
sudo fdisk -l # 列出所有磁盘, 包括逻辑盘, 没有挂载成功的
sudo df -h # 只列出挂载成功的磁盘
```

```shell
Disk /dev/sda: 6.55 TiB, 7201327243264 bytes, 14065092272 sectors
Disk model: LOGICAL VOLUME  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 262144 bytes / 786432 bytes
Disklabel type: gpt
Disk identifier: AB50FC4C-2142-47B1-8079-737EF1CC64D1

Device       Start         End     Sectors  Size Type
/dev/sda1     2048     2203647     2201600    1G EFI System
/dev/sda2  2203648     6397951     4194304    2G Linux filesystem
/dev/sda3  6397952 14065088511 14058690560  6.5T Linux filesystem

# 下面这个就是虚拟卷的位置
Disk /dev/mapper/ubuntu--vg-ubuntu--lv: 100 GiB, 107374182400 bytes, 209715200 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 262144 bytes / 786432 bytes

#  This is the path to the logical volume managed by LVM.
# ubuntu--vg is the volume group name.
# ubuntu--lv is the logical volume name.
```

### 创建逻辑卷

```shell
# 以下操作都需要root权限
vgdisplay ubuntu-vg # 查看volumne group 的信息
# 创建一个新的卷, 大小是 6.45T, 放在 ubuntu-vg 中
lvcreate -L 6.45T -n new_lv_name ubuntu-vg
# 将这个卷惊醒格式化, ext4
mkfs.ext4 /dev/ubuntu-vg/new_lv_name   # Format with ext4 filesystem
# 创建一个挂在点
mkdir /mnt/new_lv_name   # Create a mount point
# 把磁盘加到组里
mount /dev/ubuntu-vg/new_lv_name /mnt/new_lv_name   # Mount the logical volume
# 配置自盘自动挂载, 每次重启
echo '/dev/ubuntu-vg/new_lv_name /mnt/new_lv_name ext4 defaults 0 0' | sudo tee -a /etc/fstab

```



