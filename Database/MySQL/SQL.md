

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

| LIKE                    | Case insensitive exact string comparison                                           | col_name LIKE "ABC"                                                     |
| ----------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| NOT LIKE                | Case insensitive exact string inequality comparison                                | col_name NOT LIKE "ABCD"                                                |
| %                       | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name LIKE "%AT%"  <br>(matches "AT", "ATTIC", "CAT" or even "BATS") |
| _                       | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name LIKE "AN_"  <br>(matches "AND", but not "AN")                  |
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
