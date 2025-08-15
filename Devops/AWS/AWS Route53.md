
Route53 does not charge for alias queries to AWS resource
Route 53 charge for CNAME queries

TTL: DNS 记录在缓存中保存的时间. TTL 越长, 越便宜
Router53 Resolve  对 aws 里面的资源做DNS解析

Routing Policy
- simple 面向单台服务器
- failover 面向 active-passive 两台服务器
- geolocation 面向两个 Region
- geoproximity 用户真实物理距离
- latency-based
- multivalue answer 随机从 8 个 records 中选择
- weighted 总共 255. 也是不同 region 之间

A Record can be used on-premise
Alias Record can only be used in cloud

Inbound Route53 Resolver 面向 on-premise 站点的需求解析反馈 VPC 云上资源的 IP
Outbound Route53 Resolver 面向 VPC 上的请求, 对 on-premise上资源的 IP