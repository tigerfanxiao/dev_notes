
用 hybrid 方式来配置 access 口
```shell
interface GigabitEthernet 0/37
 poe enable
 switchport mode hybrid
 switchport hybrid native vlan 100
```