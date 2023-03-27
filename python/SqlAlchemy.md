
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
```python
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()  # 基类
  
class Person(Base):
	__tablename__ = "people"
	
```


## filter

```python
session.query(Person).filter(Person.lastname.Like("%An%"))

session.query(Person).filter(Person.lastname.in(['Anna', 'Mike']))
```

# alembic

```shell
alembic init migrations
```

配置 `migrations/env.py`
```python
from models import Base  
target_metadata = Base.metadata
```


如果migration已经有了， 下面用来重构数据库
```shell
alembic revision --autogenerate -m "init db"
alembic upgrade heads
```