
创建或者连接数据库

```python

import sqlite3

conn = sqlite3.connect('database_file_path')  # 如果路径下的数据库文件不存在会自己创建

cursor = conn.cursor()
cursor.execute("create table test1(t1 int, t2 varchar(40))")

result = cursor.fetchall()



 conn.close()

```


