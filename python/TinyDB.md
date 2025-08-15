
创建数据库

```python
from tinydb import TinyDB, where


db = TinyDB(os.path.join(DB_FOLDER, 'db.json'))
```