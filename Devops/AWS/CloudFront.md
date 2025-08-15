是AWS 的CDN
可以配置origin
1. S3 Bucket,
2. EC2 instance
3. Elastic Load Balancer
4. Route53

解决客户请求URL中的大小写问题
用户请求的URL中字母的大小写, 会减少资源的命中率. 用lambda@edge 将请求中的URL全部小写, 并排序