Block Storage 适用于经常变化的数据
Object Storage 适用于 write once, read more 的数据

EBS Elastic Block Store 是 EC2 的外部存储. 不会因为 EC2实例的销毁而销毁, EC2 本身也是有存储的, 称为 EC2 Instance Storage, 优势是速度快, 确定是在 EC2 销毁的时候, 会被销毁. EBS 是可以做 snapshot 的. 通常情况下EBS 和 EC2 是一对一的关系. 现在有 multi-attach EBS, 单不是所有的 EC2类型都支持

EBS 有 SSD 和 HDD 两种. SSD 可以做文件系统, 看的是 IOPS, HDD 看的是吞吐量, 500MiB/s 或者 250MiB/S


### Scale Amazon EBS Volumes

You can scale Amazon EBS volumes in two ways.

1. Increase the volume size, as long as it doesn’t increase above the maximum size limit. For EBS volumes, the maximum amount of storage you can have is 16 TB. That means if you provision a 5 TB EBS volume, you can choose to increase the size of your volume until you get to 16 TB.
    
2. Attach multiple volumes to a single Amazon EC2 instance. EC2 has a one-to-many relationship with EBS volumes. You can add these additional volumes during or after EC2 instance creation to provide more storage capacity for your hosts.

### Benefits of Using Amazon EBS

Here are the following benefits of using Amazon EBS (in case you need a quick cheat sheet).

- High availability: When you create an EBS volume, it is automatically replicated within its Availability Zone to prevent data loss from single points of failure.
    
- Data persistence: The storage persists even when your instance doesn’t.
    
- Data encryption: All EBS volumes support encryption.
    
- Flexibility: EBS volumes support on-the-fly changes. You can modify volume type, volume size, and input/output operations per second (IOPS) capacity without stopping your instance.
    
- Backups: Amazon EBS provides you the ability to create backups of any EBS volume.

When you take a snapshot of any of your EBS volumes, these backups are stored redundantly in multiple Availability Zones using Amazon S3.

Here are a few important features of Amazon EFS and FSx to know about when comparing them to other services.
- It is file storage.
- You pay for what you use (you don’t have to provision storage in advance).
- Amazon EFS and Amazon FSx can be mounted onto multiple EC2 instances.


|                                   |                     |
| --------------------------------- | ------------------- |
| Availability (%)                  | Downtime (per year) |
| 90% ("one nine")                  | 36.53 days          |
| 99% ("two nines")                 | 3.65 days           |
| 99.9% ("three nines")             | 8.77 hours          |
| 99.95% ("three and a half nines") | 4.38 hours          |
| 99.99% ("four nines")             | 52.60 minutes       |
| 99.995% ("four and a half nines") | 26.30 minutes       |
| 99.999% ("five nines")            | 5.26 minutes        |