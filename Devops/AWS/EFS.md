
为什么要用 EFS
因为在 S3 上做数据分析并不方便. 
而 EFS 可以允许多个 EC2 来访问, 并做数据分析

EFS的数据也是存在多个 AZ上的
EFS 的大小根据你存储的内容可以弹性伸缩
EFS 上的内容可以加密

Storage Class
- Standard 通过 lifecycle function 可以把数据移动到 Infrequent Access中
- Infrequent Access
