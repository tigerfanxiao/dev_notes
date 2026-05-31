
```python
from datetime import datetime, timedelta

# 返回的是datetime对象
datetime.now() 
datetime.now('UTC')

datetime.now() - timedelta(days=2)

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