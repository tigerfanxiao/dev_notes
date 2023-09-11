
普通的 ip 到 domain 的映射

# policy
- weighted policy. 当一个 domain 有多个 IP 地址时, 可以根据权重配置, 将流量引导不同的 IP 地址. 权重从 0-255
- Geolocation Policy 根据物理距离来引导流量
- Latency Policy 
- Failover Policy Route53 会检测指定目标是否还存在. 如果发现挂了, 则会把流量引入好的 ip 地址
- Multivalue answer policy 返回多个 healthy IP 地址. 如果有 unhealthy IP 则不返回


最佳实践
当一个网站从老的站点切换到新的站点时, 可以利用weighted policy把流量部分引导新的站点. 在稳定跑起来之后, 在把老站点的权重设置为 0, 完成割接



