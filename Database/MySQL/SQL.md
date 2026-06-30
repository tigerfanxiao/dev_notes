
# Concepts
- Database
	- Inside of database is tables
- Column
	- column name is lower case and underscore 
	- column is also called as field, attribute
	- 


### WHERE

| Operator                | Condition                                                                          | SQL Example                                                             |
| ----------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| =, !=, <>, <, <=, >, >= | Standard numerical operators                                                       | col_name != 4                                                           |
| BETWEEN … AND …         | Number is within range of two values (inclusive)                                   | col_name BETWEEN 1.5 AND 10.5                                           |
| NOT BETWEEN … AND …     | Number is not within range of two values (inclusive)                               | col_name NOT BETWEEN 1 AND 10                                           |
| IN (…)                  | Number exists in a list                                                            | col_name IN (2, 4, 6)                                                   |
| NOT IN (…)              | Number does not exist in a list                                                    | col_name NOT IN (1, 3, 5)                                               |
| LIMIT                   | First rows                                                                         |                                                                         |
|                         |                                                                                    |                                                                         |
### LIKE

| LIKE     | Case insensitive exact string comparison                                           | col_name LIKE "ABC"                                                     |
| -------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| NOT LIKE | Case insensitive exact string inequality comparison                                | col_name NOT LIKE "ABCD"                                                |
| %        | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name LIKE "%AT%"  <br>(matches "AT", "ATTIC", "CAT" or even "BATS") |
| _        | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name LIKE "AN_"  <br>(matches "AND", but not "AN")                  |
### Sorting

|          |                   |                                                      |
| -------- | ----------------- | ---------------------------------------------------- |
| DISTINCT | discard duplicate | Should be put on the beginning and apply to all cols |
| ORDER BY |                   | ORDER BY col ASC/DESC                                |
| LIMIT    |                   |                                                      |
| OFFSET   | next rows         |                                                      |
|          |                   |                                                      |


### JOIN

student，  course 都是表

`student` Table

| id  | first_name | last_nane |
| --- | ---------- | --------- |
| 001 | xiao       | fan       |
`course` Table

| id  | name |
| --- | ---- |
| 01  | math |
`student_course` Table

| student_id | course_id |
| ---------- | --------- |
| 001        | 01        |


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


### Not Null

```sql

select * from table where col is not Null

```

### Operation

Division
```sql
SELECT title, (domestic_sales + international_sales) / 1000000 AS gross_sales_millions
FROM movies
  JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```

Even
```sql
SELECT a.title, a.year
FROM movies as a
where year % 2 =0
```

# group by having

|     |         |
| --- | ------- |
| AVG | average |
| SUM | sum     |

## Row Operation

### Insert Row

```sql
INSERT 
INTO boxoffice (movie_id, rating, sales_in_millions) 
VALUES (1, 9.9, 2);
```
检查后插入
```sql
--- 如果存在就不插入
INSERT OR IGNORE INTO yourtable (field1,field2) VALUES(1,2)；
--- 如果存在就替换
INSERT OR REPLACE INTO yourtable (field1,field2) VALUES(1,2);
```

### Edit Row
```sql
UPDATE mytable 
SET <col_1> = <value>, <col_2> = <value>, 
WHERE condition;
```

### Delete Row

```sql
DELETE FROM mytable 
WHERE condition;
```

## Table Operation
### Create Table
```sql
CREATE TABLE IF NOT EXISTS mytable 
(<col_1> <data_type> <constraint> DEFAULT <default_value>,  <col_2> <data_type> <constraint> DEFAULT <default_value>, … );
```

#### Data Type

| Data type                                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `INTEGER`, `BOOLEAN`                                 | The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1.                                                                                                                                                                                                                                                          |
| `FLOAT`, `DOUBLE`, `REAL`                            | The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value.                                                                                                                                                                                                                                              |
| `CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT` | The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.<br><br>Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables. |
| `DATE`, `DATETIME`                                   | SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones.                                                                                                                                                                                                                                                                               |
| `BLOB`                                               | Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them.                                                                                                                                                                                                                                                           |
#### Constraint

| Constraint           | Description                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PRIMARY KEY`        | This means that the values in this column are unique, and each value can be used to identify a single row in this table.                                                                                                                                                                                                                                                                           |
| `AUTOINCREMENT`      | For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases.                                                                                                                                                                                                                                                  |
| `UNIQUE`             | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table.                                                                                                                                            |
| `NOT NULL`           | This means that the inserted value can not be `NULL`.                                                                                                                                                                                                                                                                                                                                              |
| `CHECK (expression)` | This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc.                                                                                                                                                                         |
| `FOREIGN KEY`        | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.  <br>  <br>For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the `FOREIGN KEY` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list. |
### Add Column

```sql
ALTER TABLE <table_name> 
ADD <col_name> <col_data_type> <contrainst> <default_value>;
```

### Delete Column
```sql
ALTER TABLE <table_name>  
DROP <col_name>;
```
### Delete Table

```SQL
DROP TABLE IF EXISTS table_name  -- 删除表， 表结构一起删除。 
```


# Foreign Key


### 基本语法练习
```sql
CREATE TABLE students ( 
	id INT PRIMARY KEY, 
	name VARCHAR(50), 
	age INT, 
	gender CHAR(1), 
	class_id INT 
); 

INSERT INTO students VALUES 
(1,'张三',18,'M',1), 
(2,'李四',19,'M',2), 
(3,'王五',18,'F',1), 
(4,'赵六',20,'F',2), 
(5,'钱七',17,'M',1), 
(6,'孙八',19,'F',3);

CREATE TABLE classes ( 
	id INT PRIMARY KEY, 
	class_name VARCHAR(50) 
); 

INSERT INTO classes VALUES 
(1,'一班'), 
(2,'二班'), 
(3,'三班');


```
问题
```sql
-- 1. 查询所有学生信息
select * from students;

-- 2. 只查询 学生姓名、年龄 两列，不要全部字段。
select name, age 
from students;

-- 3. 查询 **年龄大于 18 岁** 的所有学生信息。
select * from students
where age > 18;

-- 4. 查询：性别为女 (F) 的所有学生信息
select * from students
where gender = 'F';

-- 5.查询 一班 (class_id = 1) 的所有学生
select * from students
where class_id = 1;

-- 6. 查询 年龄 **18 ~ 20 之间** 的学生（包含 18、20）
SELECT * FROM students
WHERE age BETWEEN 18 AND 20;

-- *7. 查询 **姓张** 的学生（模糊查询）
SELECT * FROM students
WHERE name LIKE '张%';

-- 8. 题目：查询所有学生，**按年龄从小到大排序**
SELECT * FROM students
ORDER BY age ASC;

-- 9. 题目：查询所有学生，**年龄从大到小，年龄相同再按姓名从小到大排序**
SELECT * FROM students
ORDER BY age DESC, name ASC;


-- *10. 查询学生表里**所有不重复的年龄**
SELECT DISTINCT age FROM students;


-- 11. 查询**年龄最大的 2 个学生**, 关键字：`LIMIT`
SELECT * FROM students
ORDER BY age DESC
LIMIT 2;

-- 12. 使用聚合函数，查询**学生总人数**
SELECT COUNT(id) AS student_count FROM students;

-- 13. 查询全体学生**平均年龄**
SELECT AVG(age) AS average_ages FROM students;

-- *14. 查询学生 **最大年龄、最小年龄**
SELECT MAX(age), MIN(age) FROM students;

-- *15. 统计**每个班级**的学生人数
SELECT class_id, COUNT(id) AS student_num
FROM students
GROUP BY class_id;

-- 16. 学生姓名 + 对应班级名称
SELECT a.name, b.class_name 
FROM students AS a, classes AS b
WHERE a.class_id = b.id;
-- * Inner Join写法
SELECT a.name, b.class_name
FROM students AS a
INNER JOIN classes AS b
ON a.class_id = b.id;


-- *17. 查询**每个班级名称 + 对应学生人数**
SELECT 
	b.class_name, 
	COUNT(a.id) AS student_num
FROM students AS a
INNER JOIN classes AS b
on a.class_id = b.id
GROUP BY b.id


-- 18. 基于正确的 17 题语句，增加条件：班级人数 > 2
SELECT 
	b.class_name, 
	COUNT(a.id) AS student_num
FROM students AS a
INNER JOIN classes AS b
on a.class_id = b.id
GROUP BY b.id
HAVING COUNT(a.id) > 2;


-- *19. - 只统计 **18 岁及以上** 的学生
-- 按班级名称分组, 算出每个班级的大龄学生人数, 只保留 **人数≥2** 的班级
SELECT 
	b.class_name, 
	COUNT(a.id) AS student_num
FROM students AS a
INNER JOIN classes AS b
on a.class_id = b.id
WHERE a.age >= 18
GROUP BY b.id, b.class_name -- 凡是写在Select中的非聚合字段必须出现在group by中
HAVING COUNT(a.id) > 2
LIMIT 2;

```