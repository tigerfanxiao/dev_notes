

可以使用context manager 自动acqure和release object 


### SQLite数据库连接

```python

class SqliteManager:  
	def __init__(self, sqlite_file_path):  
		self.sqlite_file_path = sqlite_file_path  
		self.conn = None  
		self.cursor = None  
  
def __enter__(self):  
	self.conn = sqlite3.connect(self.sqlite_file_path)  
	self.cursor = self.conn.cursor()  
	return self.cursor  
  
def __exit__(self, exc_type, exc_value, exc_tb):  
	self.cursor.close()  
	self.conn.close()

```


调用
```python

from writable import WritableFile

with WritableFile("hello.txt") as file:
   file.write("Hello, World!")

```