
# 任务批次
- [ ] 0_100
	- [ ] Spain_OSP_NetChange_Stage1_1_102
	- [ ] Spain_OSP_NetChange_Stage2_1_102
	- [ ] Spain_OSP_NetRollBack_Stage1_1_102
	- [ ] Spain_OSP_NetRollBack_Stage2_1_102
- [ ] 

# 遗留问题

- [x] display isis route 208 verbose 问题
- [x] 借用 display bgp vpnv4 all routing-table statistics 
- [ ] 构建Stage1的样例输入
- [ ] 当前设备有多个isis进程, 需要注意




# 现网检查
1. 是否所有的设备都只有一个network-entity的配置, 不存在多个

# 整体策略
两次下发配置

下发的次序很重要， 首先是增， 然后是删

分割正则
```shell
(?=keyword)

(regex)content(regex)
$1
$2
```

1. 该network-entity

前后对比
1. 新增network-entity
2. uptime 



1. HMO客户端
2. 290台 - TP1=100， 后续200台
   2702总共 - 17晚上
3. 设备的清单 100台： 设备名，IP地址，连接方式（ssh/telnet）， 设备的登录名和密码
4. 两次下发的前后状态 4个文件

注意： 
因为同一条命令不能写两个不同的检查和对比规则。 所以同一条命令在两次下发中要使用不同的形式


# HMO结构

## 场景
### 执行
* Spain_OSP_Network_Entity_Change_Stage1
* Spain_OSP_Network_Entity_Change_Stage2
### 回退
* Spain_OSP_Network_Entity_Rollback_Stage1
* Spain_OSP_Network_Entity_Rollback_Stage2

## Spain_OSP_Network_Entity_Change_Stage1
### Precheck1
```
display current-configuration configuration # 备份
display isis lsdb verbose 备份
display ip routing-table statistics # 对比
display ip routing-table all-vpn-instance statistics  # 对比
display bgp vpnv4 all routing-table statistics # 对比
display ip routing-table protocol isis
display isis route 208 verbose
display isis statistics
display isis lsdb
display isis system-id conflict
display isis interface
display health verbose
display alarm all
display alarm active
display version
display interface brief
display isis peer
display isis peer verbose
display isis interface verbose
display bgp peer
display device
display device pic-status
display startup
display patch
display module
```

### Config1
变量
```
${config_add_new_net}
```
配置
```
system-view
isis 208
network-entity 49.2208.0102.5500.9169.00
commit
return 
save
Y
```
### Postcheck1

```
display current-configuration configuration isis | include network  # 检查
display isis lsdb verbose 208 | include AREA  # 检查
display ip routing-table statistics # 对比
display ip routing-table all-vpn-instance statistics # 对比
display bgp vpnv4 all routing-table statistics # 对比
display isis route 208 verbose
display isis lsdb
display isis statistics
display isis system-id conflict
display isis lsdb verbose
display isis interface
display health
display health verbose
display alarm all
display alarm active
display version
display interface brief
display isis peer
display isis peer verbose
display isis interface verbose
display bgp peer
display device
display device pic-status
display startup
display patch
display module
```

## Spain_OSP_Network_Entity_Change_Stage2
### Precheck2
```
display current-configuration configuration isis  # 备份
display isis lsdb verbose # 备份
display ip routing-table statistics  # 对比
display ip routing-table all-vpn-instance statistics  # 对比
display bgp vpnv4 all routing-table statistics # 对比
display current-configuration
display ip routing-table protocol isis
display isis route 208 verbose
display isis lsdb
display isis statistics
display isis system-id conflict
display isis interface
display health
display health verbose
display alarm all
display alarm active
display version
display interface brief
display isis peer
display isis peer verbose
display isis interface verbose
display bgp peer
display device
display device pic-status
display startup
display patch
display module
```
### Remove Old NET
变量
```
${config_remove_old_net}
```
配置
```
system-view
isis 208
undo network-entity 49.1208.0102.5500.9169.00
commit
return 
save
Y

```
### Postcheck2
```
display current-configuration configuration isis  # 检查
display isis lsdb verbose 208 # 检查
display isis peer verbose 208 # 检查
display isis route 208 verbose # 检查
display health # 检查
display ip routing-table statistics # 对比
display ip routing-table all-vpn-instance statistics # 对比
display bgp vpnv4 all routing-table statistics # 对比
display current-configuration configuration
display ip routing-table protocol isis
display isis lsdb
display isis statistics
display isis system-id conflict
display isis lsdb verbose
display isis interface
display health verbose
display alarm all
display alarm active
display version
display interface brief
display isis peer
display isis peer verbose
display isis interface verbose
display device
display device pic-status
display startup
display patch
display module
```


## Spain_OSP_Network_Entity_Rollback_Stage1
```
${config_add_old_net}
```


## Spain_OSP_Network_Entity_Rollback_Stage2
```
${config_remove_new_net}
```


# 规则


## 开发记录

|阶段|命令|规则|进度|
|--|--|--|--|
|第二次|display current-configuration configuration isis| 对比 | |
|第二次|display bgp vpnv4 all routing-table statistics|对比||
|第二次|display current-configuration configuration isis|检查||
|第二次|display current-configuration configuration isis include|检查||
|第二次|display health|对比||
|第二次|display ip routing-table all-vpn-instance statistic|对比||
|第二次|display ip routing-table statics|对比||
|第二次|display isis lsdb verbose 208 include|检查||
|第二次|display isis lsdb verbose 208|对比||
|第二次|display isis peer verbose 208|对比||
|第二次|display isis route 208 verbose|检查||





### display current-configuration configuration isis

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|leaf|entity2208|`network-entity\s+\d+\.(2208)\.\d+\.\d+\.\d+\.\d+`||
|list|count_network_entity||`(?=network-entity)`|
|leaf|entity|`(network-entity\s+\d+\.\d+\.\d+\.\d+\.\d+\.\d+)`||

第二阶段
```shell
# 前检查
display current-configuration configuration isis | include network
\s*display\s+current-configuration\s+configuration\s+isis\s+\|\s+include\s+network\s*$

# 后检查
display current-configuration configuration isis
```

预期回显

```shell
<BX9300RA3>display current-configuration configuration isis
#
isis 12479
 is-level level-1
 cost-style wide
 timer lsp-generation 1 50 50 level-1
 flash-flood 15 level-1
 bandwidth-reference 60000
 auto-cost enable
 network-entity 49.1208.0102.5500.9169.00
 network-entity 49.2208.0102.5500.9169.00
 import-route direct level-1 tag 900 route-policy DIRECT_TO_ISIS
 area-authentication-mode md5 cipher %^%#$u*%8Z6M)4y3e;Qs#B_5suTHLHFB_=Pg5&.H(Kx6%^%#
 timer spf 1 50 50
 traffic-eng level-1
 local-mt enable
 set-overload on-startup 300
#
```

### display current-configuration configuration isis | include network
第一次下发
|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|entity4922||`(?=network-entity\s+49\.2208)`|
|leaf|entity2208|`(network-entity\s+\d+\.2208\.\d+\.\d+\.\d+\.\d+)`||
|leaf|entity49x2||`(?=network-entity\s+49\.[0-13-9]208)`|
|leaf|entityx208|`(network-entity\s+49\.[0-13-9]208\.\d+\.\d+\.\d+\.\d+)`||


命令获取正则
```shell
\s*display\s+current-configuration\s+configuration\s+isis\s*$ 
```

检查规则
1. 2208一定存在
对比规则
1. 从2条变成1条

## display isis lsdb verbose 208

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|network_entity|| `(?=AREA)`|
|leaf|entity| `AREA\s+ADDR\s+49\.(\d+)`||


## display isis lsdb verbose 208 | include AREA


|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|area4922|| `(?=AREA ADDR\s+49\.2208)`|
|leaf|area2208| `(AREA ADDR\s+49\.2208)`||
|list|area49x2|| `(?=(AREA ADDR\s+49\.[013-9]208))`|
|leaf|areax208| `(AREA ADDR\s+49\.[013-9]208)`||


```shell
display isis lsdb verbose 208 | include AREA
display isis lsdb verbose 208
```


## display health

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|slot|`\d+\s+(\S+)\s+(\d+)%\s+\S+\s+\S+`||
|leaf|cpu_usage|`$1`||

## display ip routing-table statistics 
```shell
\s*display\s+ip\s+routing-table\s+statistics\s*$
```

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|total_row|`Total\s+(\d+)\s+\S+\s+\S+\s+\S+\s+\S+`||
|leaf|total_number|`$1`||

## display ip routing-table all-vpn-instance statistics

```shell
\s*display\s+ip\s+routing-table\s+all-vpn-instance\s+statistics\s*$
```

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|total_row|`Total\s+(\d+)\s+\S+\s+\S+\s+\S+\s+\S+`||
|leaf|total_number|`$1`||



## dislay isis route 208 verbose
```shell
\s*dislay\s+isis\+route\s+208\s+verbose\s*$
```


|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|peer||`(?=IPV4\sDest)`|
|leaf|age|`Age\s+:\s+(\d+)`||

## display isis peer verbose 208

```shell
\s*display\s+isis\s+peer\s+verbose\s+208\s*?
```

|类型|信息项|匹配正则|分割正则|
|--|--|--|--|
|list|peer||`(?=Uptime)`|
|leaf|uptime_hour|`Uptime\s+:\s*(\d+)h`||

