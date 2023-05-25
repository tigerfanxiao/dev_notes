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

### 改造df
```python
# 去掉 ID 列
df = df.loc[:, df.columns != 'ID'] 

# 取出部分列
df = df[["Device Name", "Product Name", "Product Version"]] 
```

# 修饰

```python
```python
df = df.fillna('')  # 把na都变成空字符
```

```


# 统计

```python
df['field'].mean()  # 平均值

df.groupby('Year')['Height'].mean()  # group by
```

### 合并两个 DF
```python
# combine two dataframes, ignore index， 两个dataframe结构一致
df_interface = pd.concat([df1, df2], ignore_index=True)


# join 两个df

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