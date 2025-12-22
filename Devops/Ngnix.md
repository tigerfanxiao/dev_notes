web-components:
- Forward Proxy 站在用户的前面, 替用户发出请求Internet. 
	- Access Control: Block access to certain website, or restrict internet usage within a company
	- Security: Scan for any virus and block
	- Monitoring: In theory, it could log employee's web activity
	- Caching Response
- Reverse Proxy, 站在服务器的前面, 替服务器去接受来自Internet的用户请求
	- Load Balancer 站在服务器的前面, 将流量分给不同的服务器
	- Protect Servers: Expose only one proxy server as entry point. minimized exposure. Configure security measures on the proxies
	- Ensure SSL encryption is enabled 
	- Caching: 加速用户访问
	- Compression and Segmentation


Load balancing method
- Least connection: Routes traffic to the server with the fewest active connection

