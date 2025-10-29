Route 53 不仅提供域名注册, 还提供域名解析服务. 本质上他是一个保存着域名和ip地址映射关系的数据库
AWS managed DNS. 
Global Service
Database replicated between region

# Hosted Zone
hosted zone 指域名和ip地址映射关系数据库, 需要把它和多个VPC关联起来. 即VPC可以通过它来做域名解析
private hosted zone 是说如果这个域名只有内网使用, 就只需要 private hosted zone, 然后把这个private hosted zone 可以和多个vpc关联


实例
下图可以看见多个vpc之间互访, 用private hosted zone来做域名解析
![[Pasted image 20231026075855.png]]
# policy
- weighted policy. 当一个 domain 有多个 IP 地址时, 可以根据权重配置, 将流量引导不同的 IP 地址. 权重从 0-255
- Geolocation Policy 根据物理距离来引导流量
- Latency Policy 
- Failover Policy Route53 会检测指定目标是否还存在. 如果发现挂了, 则会把流量引入好的 ip 地址
- Multivalue answer policy 返回多个 healthy IP 地址. 如果有 unhealthy IP 则不返回


最佳实践
当一个网站从老的站点切换到新的站点时, 可以利用weighted policy把流量部分引导新的站点. 在稳定跑起来之后, 在把老站点的权重设置为 0, 完成割接



