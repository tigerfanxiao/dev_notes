# 概念

## Dataframe 和 Series

读取csv

```python

import pandas as pd

# 指定读取部分列
specific_cols_names = ['col1', 'col2']
# 对于一些欧洲字符
df = pd.read_csv(csv_file_path,  encoding='iso8859-1', usecols=specific_cols_names)

# get how much rows and columns 
print(df)

# check all the columns name
df.dtypes
# 查看有多少单元格
df.size
# 查询列数和行数
df.shape

# 查看头部
df.head()

# 查看有多少行
len(df.index)

```

### 过滤 DF
```python
# 去掉 ID 列
df = df.loc[:, df.columns != 'ID'] 

# 取出部分列
df = df[["Device Name", "Product Name", "Product Version"]] 


df.append(row, ignore_index=True)  # 增加一个字典来增加一行
```

面向对象
```python
# 把df转变为字典， 且字典的key是列名
user_input_df.T.to_dict()
```

迭代
```python

# 迭代df

df = pd.DataFrame({'c1': [10, 11, 12], 'c2': [100, 110, 120]})
df = df.reset_index()  # make sure indexes pair with number of rows

for index, row in df.iterrows():
    print(row['c1'], row['c2'])


# 迭代 Series
for table_name in all_tables_df['name']:  # 这里all_tables_df['name']是Series
	print(table_name)
```

### 运算
```python
# 将计算后的列插入df
# 这里函数 get_new_tunnel_id 也是一个df
te_tunnel_df['new_tunnel_id'] = te_tunnel_df['Tunnel ID'].apply(get_new_tunnel_id)
```


# 查找


```python
# 查找单个元素
df.loc[df['column_name'] == some_value]  

# 多个条件
df.loc[(df['column_name'] >= A) & (df['column_name'] <= B)]

# 定位一个df中第一行的某个元素
df.iloc[0]['colname']  # 这里0是行数

# 判断df是否数据为空
df.empty
```


# 修饰
### 填充

```python
df = df.fillna('') 

# 删除 id列
df = df.drop(columns=["id"])

 ```


# 统计

```python
df['field'].mean()  # 平均值

df.groupby('Year')['Height'].mean()  # group by
```

### 合并两个 DF
```python
# combine two dataframes, ignore index， 两个dataframe结构一致
# 纵向叠加两个表
df_interface = pd.concat([df1, df2], ignore_index=True)


# join 关联两个表
pd.merge(df1, df2, on='business_id', how='outer')
```




# 构建新的DF



# 读取sqlite

```python
import pandas as pd

engine = create_engine(f'sqlite:///{DB_FILE}')
table_df = pd.read_sql_table(  
	'license_item',  
	con=engine  
	)
```

从DF转到SQLite

```python

conn = sqlite3.connect(sqlite_path)  
# head = 0 将excel表头保存为sqlite中的表头
# head = None 把excel表头保存为表的第一行
dfs_dict = pd.read_excel(xlsx_path, sheet_name=None, header=0, dtype=str)  

for sheet, df in dfs_dict.items():  # 遍历所有的sheet
	df.to_sql(sheet, con=conn, if_exists='replace', index=False)  # 将df转到SQLite
	print(f'insert table {sheet} {df.shape[0]} rows')  

conn.close()
```


# 透视表
本质上要找两个categorize的分类。 
index是左边纵向的序列， columns是上面的分类， 合计的计算公式可以自己定义， 不定义的话， 默认是mean
* 生成pivot表之后， 上面会有multindex， 不能和直接和其他的df进行关联
* 需要去掉一层level后才能关联

```python
pivot_table = df.pivot_table(index='Device Name', columns=['Item Name'], values=['Value'], aggfunc=lambda x: x)

# 去掉上层的level
pivot = pivot_table['Value']
```

# 输出
### excel
```python
# 输出到一个新的excel文件
df.to_excel(path, sheet_name='sheet name', index=False)


# 输出到同一个excel文件， 多个sheet
with pd.ExcelWriter(file_path) as writer:
	data_frame1.to_excel(writer, sheet_name, index=False)
	data_frame2.to_excel(writer, sheet_name, index=False)

```