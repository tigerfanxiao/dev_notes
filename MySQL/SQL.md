


# 插入数据

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

# 关联查询

### 多个表关联

student， student_course, course 都是表

```sql
SELECT
	student.first_name,
	student.last_name,
	course.name
FROM student
JOIN student_course
	ON student.id = student_course.student_id
JOIN course
	ON course.id = student_course.course_id;
```

# 操作表本身

```SQL
DROP TABLE table_name  -- 删除表， 表结构一起删除。 
```