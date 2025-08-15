IOPS Input/output Operations per Second 表示的是存储的读写速度. 
如果一个应用在 1 分钟内要写 60 次硬盘. 那他的 IOPS 就是 1
# Direct Attached Storage
类似于硬盘直接连接到计算节点上. 但是一个硬盘只能连接一个计算节点(node)

# File Storage
一般也称为 NFS Network File Storage 一般是基于以太网 Ethernet 为传输介质的共享存储. 
特点是收到介质的影响, 速度会比较慢. 但是文件本身不会被分割. 
可以用于多个 Application 共享的环境
常见的用例有
- Department File share 
- Landing zone of files
- Repository of files

# Block Storage
 一般有NAS, SAN Storage Area Network, 一种基于光纤为传输介质的共享存储. 一般用于数据库
注意: 
File Storage 支持并发, 多人同时访问, 但是 block Storage 不支持并发
# Object Storage
计算节点和存储是通过 API 调用实现的. 相比其他存储方式, 这种速度最慢, 有点是可以无线扩展. 
长用于, 不需要对文件进行频繁修改的场景

# CDN
Content Delivery Network
网络的延迟是距离产生的. 从欧洲到中国的延迟 200ms-300ms 是正常的. 要解决这问题, 增大带宽不能解决. 带宽只是说每个人用的带宽增加了. 要解决延迟的问题, 还是需要通过 CDN 来解决. 


