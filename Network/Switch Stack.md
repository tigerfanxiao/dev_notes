### 堆叠的方式
1. 堆叠卡
2. 堆叠线
3. 使用普通的业务口堆叠

注: 一般建议购买专业的堆叠线

堆叠线也安装速度和长短区分, 目前可以采购40Gbps的堆叠线

一般用上行接口做堆叠, 上行端口的速率高

一个逻辑堆叠口, 需要两根连线
![[Pasted image 20230914153040.png]]



### 业务口做堆叠配置
1. 在设备上先做配置. 不需要连线. 注意保存其他配置
2. 连线
3. 设备会自动重启. 注意保存配置


缺点: 浪费两个业务口.

```shell
# 创建一个虚拟的堆叠口
interface stack-port 0/1
port interface gigabitethernet 0/0/23 enable  # 使用23号业务口做堆叠
# 使用两个堆叠口
interface stack-port 0/2
port interface gigabitethernet 0/0/24 enable # 使用24号业务口


# 在另一台设备做相同的配置
```