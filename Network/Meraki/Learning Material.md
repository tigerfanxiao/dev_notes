
https://community.meraki.com/t5/Learning-Hub/ct-p/hub

用 fanxiaofanxiao2005@163.com 注册的

# Product
安全产品 MX, 在 dashboard 中称为 appliance


# firmware
Firmware rollout process has three stages:
1. Beta
2. Stable Release Candidate (RC)
3. Stable 
固件升级可以在 14 天之内rollback


# Group Policy
在某个时间段, 对一些用户进行流量整形
对 MX Security Appliance 来说, 可以通过 vlan 和 client 来做

对自己的 Inventory 可以做两种 tag
1. Network Tag
2. Device Tag

# Event Log vs Change Log


Event Log 是在 network wide 中的
Change Log 是在 Organization 中的

### Alert
network wide - Alert 里配置 alert 发给谁
organization 中的 alert 里查看所有的 alert, 也称为 alert hub

webhook 是可以定制的 json 结构


# Trouble Shooting

Switch 特有的工具
- cycle port 是重启接口
- cable test

Help 的地方有 firewall rule 用于控制设备和 Meraki Cloud 的向上链路


### Security Appliance

- **AMP:** Advanced Malware Protection (AMP) is a Cisco integration with the MX that scans for known malware files and signatures.
- **IDP and IPS:** Intrusion Detection (IDS) and Intrusion Prevention (IPS) use the Cisco Snort engine to search for known malicious signatures and traffic patterns.

Security Center 是没有买吗?

防火墙的作用
- Layer 3 and 7 firewall rules
- Traffic shaping policies
- Security and content filtering
- Bandwidth limits

在 Meraki 中 site-to-site vpn 叫 auto-vpn

client vpn有
- cisco anyconnect 
- Meraki Client VPN 原生支持 2 层 vpn, L2TP

vpn hub 和 spoke 的区别
如果 MX 是扮演 hub 的角色, 就是说 spoke 可以链接他, 他也可以链接其他 hub
如果 MX 是扮演 spoke 的角色, 那就是说别的 spoke 不可以链接他, 他只能链接到 hub

| Hub Advantages                                                                                                                                                                                                    | Spoke Advantages                                                                                                                                                            |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - Act as central routing point in topology<br>    <br>- Builds VPN tunnels to both hubs and spokes<br>    <br>- Can be utilized as an “exit” point in the VPN topology<br>    <br>- Allows for full mesh topology | - Fewer routing decisions<br>    <br>- Only builds tunnels to hubs<br>    <br>- Less resource intensive<br>    <br>- Can select hub appliance as exit point in VPN topology |

![[Pasted image 20240512233550.png]]