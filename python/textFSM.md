

### 多状态
转移到item状态， 可以去掉不匹配表头， 在结尾写了 Record时才能匹配记录


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


```python
def parse_table_test():  
temp = TextFSM(open(template))  
header = temp.header  
print(header)  
fsm = temp.ParseText(content)  # 返回列表  
my_dict = temp.ParseTextToDicts(raw)  # 返回字典
print(temp.states)  # 可以有多状态

```