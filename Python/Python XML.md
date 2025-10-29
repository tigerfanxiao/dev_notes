
![[Pasted image 20230722210107.png]]
一个 xml文件只有一个 root
每个 xml 有一个 name space 只要保证在文件内唯一
attribute 单个标签内
value 两个便签内的

如果在一个 xml 中出现两个 root, 就需要用 namespace 分开
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- Inventory XML Data -->
<inventory xmlns="https://www.cisco.com/ns/routers"==>
    <device name="csr1kv1">
        <osversion>16.09</osversion>
        <uptime>2 days</uptime>
        <vendor>cisco</vendor>
        <serial>XB968761</serial>
        <snmp name="public" permission="ro"/>
        <snmp name="private" permission="rw"/>
    </device>
</inventory>
<inventory xmlns="https://www.dell.com/ns/servers"==>
    <server name="Compute_SRV">
        <dist>Ubuntu</dist>
	   <vendor>Dell</vendor>
	   <model>XP720</model>
	   <license>Enterprise</license
        <interfaces>4</interfaces>
        <serial>183AF789</serial>
    </server >
</inventory>
```

在 root 节点定了两个 name space, 用 prod 来做第二个 name space 的别名. 第一个 name space 就是默认的 name space
```xml
<?xml version="1.0"?>
<inventory xmlns="https://www.cisco.com/ns/routers"
           xmlns:prod="https://www.cisco.com/ns/routers/prod"
           xmlns:dev="https://www.cisco.com/ns/routers/dev">
    <prod:device name="csr1kv1">
        <prod:osversion>16.09</prod:osversion>
        <prod:uptime>2 days</prod:uptime>
        <prod:vendor>cisco</prod:vendor>
        <prod:serial>XB968781</prod:serial>
        <prod:snmp name="public" permission="ro"/>
        <prod:snmp name="private" permission="rw"/>
    </prod:device>
    <prod:device name="csr1kv2">
        <prod:osversion>16.09</prod:osversion>
        <prod:uptime>14 days</prod:uptime>
        <prod:vendor>cisco</prod:vendor>
        <prod:serial>XB587381</prod:serial>
        <prod:snmp name="public" permission="ro"/>
        <prod:snmp name="private" permission="rw"/>
    </prod:device>
    <dev:device name="csr1kv3">
        <dev:osversion>16.09</dev:osversion>
        <dev:uptime>182 days</dev:uptime>must        <dev:vendor>cisco</dev:vendor>
        <dev:serial>XB914781</dev:serial>
        <dev:snmp name="public" permission="ro"/>
        <dev:snmp name="private" permission="rw"/>
    </dev:device>
</inventory>
```
# python

```python
import xml.etree.Element.Tree es ET

# 读取 xml 文件
tree = ET.parse('inventory.xml')

root_data = tree.getroot()

root_data.tag # root 节点的标签

# root_data是一个可迭代的对象
for child in root_data:
	print(child.tag, child.attrib)

# 如果 xml 中有多个name space, 需要用 URI前缀来区分
for device in root_data.finall('{https://www.cisco.com/ns/routers/prod}device'):
	name = device.attrib['name']
	# 查找单个元素
	serial = device.find('{https://www.cisco.com/ns/routers/prod}serial').txt


for snmp in root_data.iter('{https://www.cisco.com/ns/routers/dev}snmp'):
	print(snmp.attrib)


```


xmltodict 包
```python
import xmltodict
import json

with open('inventory.xml') as xml_file:
	xml = xmltodict.parse(xml_file.read())

# 这是所有的attribute 值前面会有@, 可以通过 replace 来去掉
json.dumps(xml, indent=4).replace("@", "") # 将 xml 对象转为 json

# 去掉 attribue 值钱的@, 也可以在读文件的时候, 设置属性值前缀为空字符串
with open('inventory.xml') as xml_file:
	xml = xmltodict.parse(xml_file.read(), attr_prifix="")
```