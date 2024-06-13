- The fact that ELB can load balance to IP addresses means that it can work in a hybrid mode as well, where it also load balances to on-premises servers.
    
- ELB is highly available. The only option you have to ensure is that the load balancer is deployed across multiple Availability Zones.
    
- In terms of scalability, ELB automatically scales to meet the demand of the incoming traffic. It handles the incoming traffic and sends it to your backend application.


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