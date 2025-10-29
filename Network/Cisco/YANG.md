YANG 是一种专门用来描述网络的模型语言. 具体的encoding 实现, 可以是 XML 或者 JSON. 如果是 XML 就是NETCONF去通信, 如果是 JSON就用 RESTCONF 协议去通信
YANG 模型也可以大致分为两种个. 一种是Device 模型, 一种是 Service 模型. Service 模型就包括 MPLS VPN 这种网络服务

YANG 模型是树形结构的也称为schema trees

# 参考
不同厂商的 yang 模型 https://github.com/YangModels/yang
思科自己提供的 yang 模型 https://github.com/YangModels/yang/tree/main/vendor/cisco
厂商定义自己的YANG 模型的是因为, 每个厂商可能都有自己特性. 比如用在 BGP 中的 BFD, 所以对标准的 BGP 模型进行了增强或者说扩展.


### 基本元素
Leaf 是最基本的数据节点 data node. 它不允许有子节点
```shell
leaf hostname {
    type string; 
    mandatory true; 
    config true; 
    description "Hostname for the network device"; 
}
```

	1. 必须有 type
Leaf List 类似excel 中的一列元素. 但是每个元素都是一种类型的


List 包含各种元素
```shell
list vlan {
   key "id"; 
   leaf id { 
       type int; 
       range 1..4094; # 1 到 4094
   } 
   leaf name { 
       type string; 
   } 
}
```

Container 只有组织的功能, 本身没有值

```shell
container system {
    container services { 
        container snmp { 
            presence "Enables SNMP"; 
            description "SNMP service specific configuration"; 
            // more leafs, containers and stuff here... 
        } 
    } 
}
```