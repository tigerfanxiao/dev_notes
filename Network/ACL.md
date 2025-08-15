
命中了就不会往下在走了, 所以范围小的要放在前面, 否则后面的规则不生效

ACL中用的是通配符. 
1. 完全匹配用0
2. 任意选1

匹配所有偶数的ip
192.168.1.0 0.0.0.254
匹配所有基数
192.168.1.1 0.0.0.254

分类
基本ACL 2000-2999 只能匹配源IP
高级ACL 3000-3999 匹配源IP,目的IP, IP协议类型, ICMP, TCP源目的端口号
也可以给ACL起名


基本ACL例子
```
acl number 2000
rule 5 deny source 10.1.1.1 0
rule 10 deny source 10.1.1.2 0
```

高级ACL
```
acl number 3000
rule 5 permit ip source 10.1.1.0 0.0.0.255 destination 10.1.3.0 0.0.0.255
rule 10 permit tcp source 10.1.2.0 0.0.255 destination 10.1.3.0 0.0.0.255 destination-port eq 21


# 禁止ping
acl number 3000
rule 5 deny icmp source 192.168.1.2 0 destination 192.168.2.1 0
# 看网页
rule 5 permit tcp source 192.168.1.2 0 destination 192.168.2.1 0 destination-port eq 80


```


部署时, 要考虑入站和出站方向. 分析流量的方法
入站时, 先查ACL, 如果允许再查路由表
在出站时, 先查路由表, 再查ACL

```shell
在入方向
acl 2000
rule 10 deny 192.168.1.1 0.0.0.0
int g0/0/1
traffic-filter inbound acl 2000

# 在看命中的流量
dis acl 2000

```

注意: 一个接口上同一个方向, 只能有一个ACL

### ACL 默认行为
1. 当acl在应用到接口, 用来控制流量过滤时, 默认的行为是允许所有


删除规则
```shell

acl 3000
undo rule 15 # 只要写编号就行

```