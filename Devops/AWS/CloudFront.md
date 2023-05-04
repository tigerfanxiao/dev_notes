是AWS 的CDN
origin
1. S3 Bucket,
2. EC2 instance
3. Elastic Load Balancer
4. Route53

### Alias & CNAME
Alias Records have special functions that are not present in other DNS servers. Their main function is to provide special functionality and integration into AWS services. Unlike CNAME records, they can also be used at the Zone Apex, where CNAME records cannot. Alias Records can also point to AWS Resources that are hosted in other accounts by manually entering the ARN