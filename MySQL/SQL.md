

普通插入
```sql
INSERT INTO table_name VALUES (value1, value2)
INSERT INTO table_name (field1, fiele2) VALUES (value1, value2)
```


检查后插入
```sql
--- 如果存在就不插入
INSERT OR IGNORE INTO yourtable (field1,field2) VALUES(1,2)；
--- 如果存在就替换
INSERT OR REPLACE INTO yourtable (field1,field2) VALUES(1,2);
```
