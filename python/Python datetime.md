
```python
import datetime

# 返回的是datetime对象
datetime.datetime.now()  # datetime.datetime(2023, 3, 24, 9, 1, 40, 859385) # 2023-03-24 09:01:50.429635 # 打印秒后6位

```

### Compare two date
```python
# 是不是之前的时间
print(datetime.now() > datetime_obj)
```

### Calculate date
```python
# 前天此刻的时间
datetime.datetime.now() - datetime.timedelta(days=2)
# 4 个小时前
# 2024-08-23T23:11:12.799204
(datetime.datetime.now() - datetime.timedelta(hours=4)).isoformat()
```


将文本转为Datetime对象

```python
from datetime import datetime

date_str = '2023-02-28 14:30:00'
date_format = '%Y-%m-%d %H:%M:%S' 
# convert string to datetime object
date_obj = datetime.strptime(date_str, date_format) 

# convert datetime obj to string
datetime.strftime(datetime_obj, '%Y-%m-%d %H:%M:%S')


# 得到一个时区对象
gmt_1 = datetime.timezone(datetime.timedelta(hours=1))
# 把当前时间指定到某个时区
print(now.astimezone(gmt_1)) 

```

### 第三方模块

```shell
pip3 install python-dateutil
```


```python
from dateutil import parser
from datetime import datetime
strtime = str(datetime.now())

i = parser.parse(strtime)  # 将字符串转化为时间对象
```