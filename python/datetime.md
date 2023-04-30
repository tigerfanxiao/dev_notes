
```python
from datetime import datetime

# 返回的是datetime对象
datetime.now()  # datetime.datetime(2023, 3, 24, 9, 1, 40, 859385)
print(datetime.now())  # 2023-03-24 09:01:50.429635 # 打印秒后6位


# 前天此刻的时间
print(datetime.now() - timedelta(days=2))


```



### 比较两个时间
```python
# 是不是之前的时间
print(datetime.now() > datetime_obj)
```

将文本转为Datetime对象

```python
from datetime import datetime

date_str = '2023-02-28 14:30:00'
date_format = '%Y-%m-%d %H:%M:%S' 
date_obj = datetime.strptime(date_str, date_format) # convert string to datetime obn



```