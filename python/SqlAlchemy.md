
install
```shell
python -m pip install sqlalchemy
```
如果没有orm，只是手动创建一个表

```python
from sqlalchemy import (  
    create_engine,  
    MetaData,  
    Table,  
    Column,  
    Integer,  
    String,  
    ForeignKey,  
    CHAR,  
)
 # 创建一个数据库，或者连接一个数据库
engine = create_engine('sqlite:///work.db', echo=True) 
# 创建了一张表 workers
meta = MetaData()

workers = Table(  
    'workers', meta,  # 把workers注册到meta对象中
    Column('id', Integer, primary_key=True),  
    Column('name', String),  
)

meta.create_all(engine)  # 创建一个表

```


## update

## select

```python
# ins = workers.insert().values(name='ddddd')  
with engine.connect() as conn:  
    # result = conn.execute(ins)  
    stmt = update(workers).where(workers.c.name == 'ok').values(name='fanxiao')  
    conn.execute(stmt)  
    wk = workers.select()  
    conn.commit()  
    print(conn.execute(wk).fetchall())  # 打印出sql
    
```
# ORM

## 创建数据库
如果数据库文件不存在，会创建数据库文件和对应的数据库，如果数据文件已经存在，则不会创建

```python
from sqlalchemy import create_engine  
from sqlalchemy.orm import sessionmaker

DB_FILE = 'DB_FILE_PATH'
engine = create_engine(f'sqlite:///{DB_FILE}', echo=True)  # echo=True  
Session = sessionmaker(bind=engine) # 这是一个Session类

# 如果要对数据库进行方位，需要构建一个Session对象
session = Session()
# 这里的Echo是一个继承Model的类
session.query(Echo).filter(Echo.device_name == 'MA9763RT1').first()
```

## 创建表
引入一个基类， 需要创建表的对象都继承这个基类
```python
from sqlalchemy.orm import declarative_base  
from sqlalchemy import (  
    Column,  
    Integer,  
    String,  
    ForeignKey,  
    TEXT,  
    Boolean,  
)

Base = declarative_base() 
# 使用基类创建其他类
class Person(Base):
	__tablename__ = "people"
	name = Column("name", String,  primary_key=True)
	age = Column("age", Integer) 

	# 需要初始化
	def __init__(self, name, ip_address, device_type, connection, version="", username="", password=""):  
    self.name = name  
    self.ip_address = ip_address  
    self.device_type = device_type  
    self.connection = connection  
    self.version = version  
    self.username = username  
    self.password = password

	def __repr__(self):  
    return f"{self.name} {self.ip_address} {self.device_type}"
```

field的条件限制

```python
name = Column(String(25), nullable=False, unique=True)
date_created = Column(Datetime(), default=datetime.utcnow()) # 返回datetime对象

```

## 查询

```python
session.query(Person).filter(Person.lastname.Like("%An%"))

session.query(Person).filter(Person.lastname.in(['Anna', 'Mike']))
```

关联查询
```python
session.query(Echo, Device).filter(Echo.device_name == Device.name).filter(Device.tag == tag)
```
# Alembic

Alembic类似数控库的git， 每一次commit，在alembic中就是revision
## 安装Alembic
```shell
python -m pip install alembic
```

数据库初始化, 运行下面这条命令后，会在项目目录下创建migration文件夹

```shell
# 这个会在项目路径下创建一个migrations 文件夹
# 在项目的路径下会创建一个 alembic.ini文件
alembic init migrations  
```
1. 配置alembic.ini文件
```
# 如果是sqlite数据库
sqlalchemy.url = sqlite:///database/device.db
# 如果是postgresql
sqlalchemy.url = postgresql://username:password@localhost:5432/databasename
```
2. 配置 `migrations/env.py`
```python
from models import Base  # Base是在其他model中Base = declarative_base()的基类
target_metadata = Base.metadata
```
每一次commit都会留在versions目录下保存一个版本

如果migration已经有了， 下面用来重构数据库
```shell
# 应该是检查了类中的变化
alembic revision -m "init db" # 类似一个commit， -m后写commit的信息
alembic revision --autogenerate -m "init db"

# 把 version中的所有commit再跑一边
alembic upgrade heads  

# merge head
alembic merge -m "merge" <commit1> <commit2>


# 查看提交历史
alembic history
```


# Error Handling

```python
import sqlite3
from sqlalchemy import exc

session.add(echo)  # 在插入的时候， 不需要，因为是懒加载

try:    
	session.commit()  # 只有在提交的时候会报错， 
except (exc.IntegrityError, sqlite3.IntegrityError)as e:
	print("Duplicated Device Name!")
	session.rollback()
```