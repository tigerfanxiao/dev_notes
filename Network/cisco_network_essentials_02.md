# 常见protocols
| Protocol | Function   | User |
| ------------ | ------------ | ---- |
| Ethernet  | 明确了使用网卡和UTP线缆来接入设备  | NIC in LAN |
| IP  | 明确了source和destination, packet要往哪里发  | Routers in network|
| TCP  | assures reliability 对端设备是否收到了请求, 是否需要重传, 是否需要重排  | End to End Devices |
| HTTP |服务器和浏览器之间的协议, 通过HTML/CSS/JS来传输网页信息 | Client - Server |
 
# Standards Organization 
| 组织 | 标准  |
| ------------ | ------------ |
| IETF (Internet Engineering Task Force) | RFC Request for Comments  |
| IEEE  |   |
| ICANN ||

# Network Comunication Models 通信模型
## TCP/IP Model
| Layer | Protocol Stack  | Function |
| ------------ | ------------ | --|
| Application 应用层  | HTTP  | present data to user, plus encoding and dialog control|
| Transport 传输层  | TCP  | Supports communication between various devices across diverse network|
| Internet 网络层  | IP   | Determine the best path |
| Network Access 接入层 | Ethernet | controls the hardware devices and media that make up the network|

注: 一般我们说的以太网协议主要指的是物理层和数据链路层的以太网标准: 即网线, 网卡


# The Protocol Stack 协议栈


