- The fact that ELB can load balance to IP addresses means that it can work in a hybrid mode as well, where it also load balances to on-premises servers.
    
- ELB is highly available. The only option you have to ensure is that the load balancer is deployed across multiple Availability Zones.
    
- In terms of scalability, ELB automatically scales to meet the demand of the incoming traffic. It handles the incoming traffic and sends it to your backend application.


ELB Types
- Application Load Balancers
- Network Load Balancers
- Classic Load Balancers (legacy)
- Gateway Load Balancers 第三方应用

X-Forwarded-For 是 http 的 header 可以保存发送请求的用户的真实 IP 地址. 经过 ELB 透传到EC2 上
## SELECT BETWEEN ELB TYPES

Selecting between the ELB service types is done by determining which feature is required for your application. Below you can find a list of the major features that you learned in this unit and the previous.

| Feature                                                                                     | Application Load Balancer | Network Load Balancer |
| ------------------------------------------------------------------------------------------- | ------------------------- | --------------------- |
| Protocols                                                                                   | HTTP, HTTPS               | TCP, UDP, TLS         |
| Connection draining (deregistration delay)                                                  | ✔                         |                       |
| IP addresses as targets                                                                     | ✔                         | ✔                     |
| Static IP and Elastic IP address                                                            |                           | ✔                     |
| Preserve Source IP address                                                                  |                           | ✔                     |
| Routing based on Source IP address, path, host, HTTP headers, HTTP method, and query string | ✔                         |                       |
| Redirects                                                                                   | ✔                         |                       |
| Fixed response                                                                              | ✔                         |                       |
| User authentication                                                                         | ✔                         |                       |

# Error
504 Gateway Timeout 服务器故障
502 Bad Gateway ELB 无法和服务器沟通. 可能是 Security Group 的问题
503 Service Unavailable 服务器没有在 ELB 上注册