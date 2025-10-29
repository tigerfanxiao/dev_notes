
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


# MX Security Appliance
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


# Wireless

Steering from 2.4Gz to 5 Gz

Spatial stream 空间流
This transmission across multiple spatial streams to a single client is known as Single-user multiple input multiple output (SU-MIMO).

Doubling the number of spatial streams effectively doubles the available throughput. 
MIMO also allows access points to use different spatial streams to transmit to multiple clients simultaneously, known as downlink multi-user MIMO (DL-MU-MIMO).

The latest technology with wireless downlink (DL) and uplink (UL) multi-user transmissions using MIMO (DL/UL-MU-MIMO) enables multiple clients to transmit or receive simultaneously across different spatial streams.

With Meraki’s power sharing, while operating both mGig Ethernet ports:

- Two 802.3af power sources are combined to provide 802.3at power.
    
- Two 802.3at power sources are combined to provide 802.3bt power.

Meraki access points can be powered in multiple ways and will use link layer discovery protocol (LLDP) to negotiate for additional power

In addition to the tri-band radios that provide fast user connectivity, Meraki access points are furnished with a Bluetooth Low Energy radio, allowing them to listen for and locate nearby Bluetooth Low Energy devices. This feature can be used to identify and track Bluetooth Low Energy asset tags, fitness monitors, smartphones, and other Bluetooth Low Energy-enabled devices.

Meraki access points feature a dedicated Air Marshal scanning and security radio, which enables continuous RF scanning and threat mitigation (containment) without impacting data traffic or access point throughput. Flagship Meraki access points offer 2.4 GHz, 5 GHz, and 6 GHz tri-band Air Marshal WIDS/WIPS scanning, along with spectrum analysis and location analytics.

Air Marshal is Meraki’s wireless intrusion prevention system (WIPS) that strategically observes and mitigates threats occurring in the RF airspace. Once a threat has been detected, Air Marshal can enact powerful policies, including intelligent auto-disablement of access points matching a pre-defined criteria and generating different tiers of e-mail alarms based on the type of threat in your airspace.

Wi-Fi 6E takes wireless connectivity into the 6 GHz band, adding 1200 MHz of RF spectrum—more than twice the available capacity of the 2.4 and 5 GHz bands combined.

Meraki’s 6E access points offer an extended 1,200 MHz of spectrum in the U.S. and 500 MHz in Europe, with even more wireless channels—goodbye crowded RF spectrum and hello to open Wi-Fi airspace!

WPA3 offers heightened security measures against password and brute force attacks, as well as improved encryption protocols for secure device-to-device communication. While not mandatory, WPA3 is required for 6 GHz wireless deployments to mitigate potential security risks

Meraki access points include Advanced Encryption Standard (AES) hardware-based encryption, where transferred data is automatically encrypted and decrypted through an AES hardware chip built into Meraki access points. This is faster and more secure than a software-based encryption system, where data is encrypted and decrypted through a running program.

The Extensible Authentication Protocol (EAP) is used by 802.1X to create a secure tunnel between parties participating in an authentication exchange. Meraki access points support various EAP types with 802.1X authentication to connect with on-premises RADIUS servers. Meraki's cloud-first tools enable the live testing of RADIUS reachability and functionality across all access points in a Meraki network with a single click.


- Be powered with two AA batteries (up to five years), or via direct power from a USB-C connection (5V; 0.2A)
- - Proactively alert with notifications via email, SMS, push notifications, and webhooks


