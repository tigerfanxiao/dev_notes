
install
```shell
python -m pip install sqlalchemy
```
### 手动创建表非 ORM
如果没有orm，只是手动创建一个表

数据库文件一般是以 `.db` 结尾
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
engine = create_engine('sqlite:///work.db', echo=True)  # 打印日志 INFO
# 创建了一张表 workers
meta = MetaData()

workers = Table(  
    'workers', meta,  # 把workers注册到meta对象中
    Column('id', Integer, primary_key=True),  
    Column('name', String， default="default_value"),  # 设置默认值
)

meta.create_all(engine)  # 创建一个表, 如果表已经存在就不再创建

```


## Update

- `filter_by` 是 python 风格的
- `filter` 是 sql 风格的
修改表中的数据
```python
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine) # 这是一个Session类

config = session.query(Config).filter_by(device_name=ret.device_name).first()  

if config is None:
	config = Config()  # 如果元素不存在， 则新建
	session.add(config)  


# filter 是 sql 风格的

for echo in echo_list:  
	if session.query(Echo).filter(Echo.device_name == echo.device_name).first() is not None:  
		setattr(echo, 'raw_echo', echo.raw_echo)  
		session.merge(echo) # 如果元素以及存在， 则更新
	else: 
		session.add(echo)  # 如果元素不存在， 则修改
session.commit()
```

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
engine = create_engine(f'sqlite:///{DB_FILE}', echo=True)  # 在运行sql语句的时候， 会有回显
Session = sessionmaker(bind=engine) # 这是一个Session类

# 如果要对数据库进行方位，需要构建一个Session对象
session = Session()
# 这里的Echo是一个继承Model的类
session.query(Echo).filter(Echo.device_name == 'MA9763RT1').first()
```

引入一个基类， 需要创建表的对象都继承这个基类
```python
from dataclasses import dataclass
from sqlalchemy.orm import declarative_base  
from sqlalchemy import (  
    Column,  
    Integer,  
    String,  
    ForeignKey,  
    TEXT,  
    Boolean,  
    UniqueConstraint,
)
# 创建一个基类
Base = declarative_base() 
# 使用基类创建其他类
@dataclass
class Person(Base):
	__tablename__ = "people" # 表明
	name: str = Column("name", String,  primary_key=True) # 配置成主键
	age: str = Column("age", Integer, default="28")  # 默认值
	device: str = Column(Integer, ForeignKey('devices.name', ondelete="SET NULL"), unique=True) # 配置外键
	
	# 联合索引，用列名而不是字段名
	__table_args__ = (UniqueConstraint('name', 'device'),) 

	def __repr__(self):  
	    return f"{self.name} {self.ip_address} {self.device_type}"



if __name__ == '__main__':
	# 必须放在这里, 否则每次运行程序都会创建新表
	# checkfirst = True 同名表不再创建
	Base.metadata.create_all(engine, checkfirst=True)
	
```

创建条目

```python

Session = sessionmaker(bind=engine)
session = Session()

qinke = User(
			 usename='qinke',
			 password='Cisco0123',
			 realname='qingke',
			 email='colin@gmail.com'
			)
session.add(qinke)

# 删除条目
qinke.delete()

session.commit()


```



field的条件限制

```python
name = Column(String(25), nullable=False, unique=True)
date_created = Column(Datetime(), default=datetime.utcnow()) # 返回datetime对象
```

## Filter

- filter 可以使用 link 

```python
# 区分大小写匹配
session.query(Person).filter(Person.lastname.Like("%An%"))
# 字符串不区分大小写匹配
session.query(Task).filter(Task.scene_name.ilike('%clear%'))
# 字符在一个序列里
session.query(Person).filter(Person.lastname.in(['Anna', 'Mike']))
# and 条件
session.query(Person).filter(User.username.like('qin%'), User.email=='xiao@gmail.com')
# or 条件
session.query(Person).filter(or_(User.username.like('qin%'), User.email='xiao@gmail.com'))
# 查询 null 值
session.query(Person).filter(Person.age == None)

# 按照时间来过滤
date_before = datetime.now() - timedelta(days=700)
tasks = db_session.query(Task).filter(Task.create_date > date_before).all()
```

关联查询
```python
# 这里关联了Echo 和 Device两张表

entries = session.query(Echo, Device)
.filter(Echo.device_name == Device.name)  # 这里join的条件
.filter(Device.tag == tag)  # 这里是过滤的条件

# access entry
entry.Echo.device_name 
entry.Device.device_name 

```
# Alembic

Alembic类似数控库的git， 每一次commit，在alembic中就是revision
## 安装Alembic
```shell
python -m pip install alembic
```

数据库初始化, 运行下面这条命令后，会在项目目录下创建migration文件夹

```shell
# 这个会在项目路径下创建一个migrations 文件夹， 放数据库更新版本
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
# 只会创建alembic version table， 不创建模型
alembic revision -m "init db"  # 查询到多少个模型
alembic upgrade head # 给每一个模型创建一个表

# 创建模型表
alembic revision --autogenerate -m "create model tables"
alembic upgrade head

# 把 version中的所有commit再跑一边
alembic upgrade heads  

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


报错

```shell

# 如果连续运行两次 alembic revision --autogenerate -m 
# 但是没有做 alembic upgrade head
FAILED: Target database is not up to date.

# 使当前数据库为最新
alembic stamp head
```


# alembic

```shell
alembic history # 查询所有提交记录

alembic current # 查询连接数据库的情况
alembic downgrade -1 # 回退到上一个版本

# This will reset your entire database in just one statement!
alembic downgrade base && alembic upgrade head

# merge head
alembic merge -m "merge" <commit1> <commit2>
```