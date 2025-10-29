
Alarm 告警, 通过告警实现知道出现了问题
log 通过日志可以查看发生了什么 log aggregation and analysis tools
Metrics 采集 collect and visualize performance data
Events capture and process real-time events 事件


Service-Based Monitoring specific cloud services to optimize performance and ensure efficient resource utilization
Load balancing monitoring involves tracking workload distribution and identifying potential bottlenecks

Content delivery monitoring involves monitoring content delivery networks (CDNs) for efficient content distribution. Performance, latency, and cache hit rates are proactively tracked to ensure an optimal user experience. In the event of content delivery issues, troubleshooting measures can rectify the situation promptly.

Auto-scaling monitoring is essential for dynamically adjusting resource capacity in response to changing demands. By monitoring auto-scaling groups, organizations can track scaling events and evaluate the effectiveness of scaling policies. Coordination between monitoring and scaling activities ensures seamless scalability.

Infrastructure as Code (IaC) monitoring is critical for organizations utilizing automation and provisioning resources through code. Monitoring IaC deployments enables verification of infrastructure changes and detects any drift from the desired state. Configuration issues need to be identified and rectified promptly to maintain the integrity of the infrastructure.

## Tracking API Calls for Audit Purposes
AWS CloudTrail


常见的云安全问题
- Distributed Denial of Service (DDoS) Attacks
- Data Breaches 数据泄露
- Misconfigurations 错误配置, 适用 AWS Config 来审计
- Insider Threats
