
# 处理内容

```
Value Required device_name ([\w+-_]+)  
Value license (\w+)  
  
Start  
^<${device_name}>display\s+license  
^\s+Active\s+License\s+:\s+\w+   # 原来回显的内容不能省略
^\s+License state : ${license} -> Record
```


# 处理表格


### 多状态

### 案例： 去掉表头
转移到item状态， 可以去掉不匹配表头， 在结尾写了 Record时才能匹配记录

```text
Item name Item type Value Description  
-------------------------------------------------------------  
  
LANGL3VPN01 Function YES ATN 910C L3VPN Function License  
LANG2XGEPAY01 Resource 1 ATN 910C 2*10GE Port RTU  
LANG1588ACR01 Function YES ATN 910C 1588V2 ACR Function License  
LANG1588V201 Function YES ATN 910C 1588V2 Packet Synchronization Function License
```
通过设置两个状态， 然后跳转阶段来去掉表头
```
Value Required item_name (\w+)  
Value ITEM_TYPE ([\w+-]+)  
Value VALUE (\w+)  
Value DESCRIPTION ([\s\w]+)  
  
Start  
^\s+Item\s+name -> Item  
  
Item  
^\s+${item_name}\s+${ITEM_TYPE}\s+${VALUE}\s+${DESCRIPTION}\s+ -> Record

```


有两种返回方式
* 返回序列
* 返回字典序列

```python
def parse_table_test():  
temp = TextFSM(open(template))  
header = temp.header  
print(header)  
fsm = temp.ParseText(content)  # 返回列表  
my_dict = temp.ParseTextToDicts(raw)  # 返回字典
print(temp.states)  # 可以有多状态

```


```
Value Required SLOT (\w\d)  
Value SENSOR (\w+\s\w+|\w+\s\:\s\w+|\w+\:\s\w+\s\d|\w+\:\s\w+\s\S+|\w+\:\s\S+|\S+|\w+\:\s+\w+\s+\w+)  
Value CURRENT_STATE (\w+\s\w+\s\S+|\w+)  
Value READING (\d+\s+\w\s\w+|\d+\s+\w+|\d\s+\w|\d+\w+)  
Value MINOR (\d+|\d+\s+|\-\d+\s+)  
Value MAJOR (\d+|\d+\s+)  
Value CRITICAL (\d+|\d+\s+)  
Value SHUTDOWN (\d+|\d+\s+)  
  
Start  
^\s${SLOT}\s+${SENSOR}\s+${CURRENT_STATE}\s+${READING}\s+\(${MINOR},${MAJOR},${CRITICAL},${SHUTDOWN}\) -> Record  
^\s${SLOT}\s+${SENSOR}\s+${CURRENT_STATE}\s+${READING}\s+\(${MINOR},${MAJOR},${CRITICAL}\) -> Record  
^\s${SLOT}\s+${SENSOR}\s+${CURRENT_STATE}\s+${READING}\s+na -> Record
```