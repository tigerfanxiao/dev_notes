Storage Gateway 是本地 on-premise 联通到 AWS 的接口

有 3 种 Storage Gateway
- File Gateway 给你 SMB 和NFS接口联通到 S3
- Tape Gateway 磁带
- Volume Gateway 在你本地用iSCSI 块存储, 把全量数据异步备份到 S3, 但是你经常访问的数据被 cache 在本地
