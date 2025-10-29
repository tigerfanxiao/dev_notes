# Concepts

## `Dataframe` & `Series`
## read files
### read csv
```python

import pandas as pd

# 指定读取部分列
specific_cols_names = ['col1', 'col2']
# 对于一些欧洲字符
df = pd.read_csv(csv_file_path,  encoding='iso8859-1', usecols=specific_cols_names)
wine_reviews = pd.read_csv("csv_filepath", index_col=0)


# 输出到 csv
df.to_csv('path')
```

### read excel
```python
excel_data_df = pandas.read_excel('records.xlsx', sheet_name='Employees')
```

## Dataframe
### DataFrame meta info
```python

# get how much rows and columns 
print(df)

# check all the columns name
df.dtypes

# 查看有多少单元格
df.size
# 查询列数和行数
df.shape
# 获得列名序列
list(df.columns)

# 查看头部10 行数据
df.head()

# 查看有多少行
len(df.index)
```

### Create DF
从字典构建df, 本质上是按照列来构建的
```python
df1 = pd.DataFrame({'A':[3,4],'B':[5,6]})
```

| index | A   | B   |
| ----- | --- | --- |
| 0     | 3   | 5   |
| 1     | 4   | 6   |
可以给每一行加一个 index
```python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

|           | Bob           | Sue          |
| --------- | ------------- | ------------ |
| Product A | I liked it.   | Pretty good. |
| Product B | It was awful. | Bland.       |
从序列构建df
```python
# 构建只有一列的df
PATH_LIST = 'W-2_LTE,W-2_LTE_PRT,W-4_5G,W-4_5G_PRT'.split(',')
path_type_df = pd.DataFrame(PATH_LIST, columns=['path_type'])  # 需要定义列名 path_type
```

用字典的方式给df增加一行
```python
# 增加一个字典来增加一行
df.append({'col1': 'val1', 'col2': 'val2'}, ignore_index=True)  
```

用Series给df增加一行
```python
df = pd.DataFrame({'name': ['John', 'Mike'], 'age': [25, 30]})
# new_row 是 series
new_row = pd.Series(['Sam', 31], index=df.columns)
```

获得一列数据
```python
df['col1'].values.tolist()
```


### Create Series
从序列构建series
```python
# 从序列构建Series
sr = pd.Series([1, 2, 3], name='my_series')
''' 
0    1
1    2
2    3
dtype: int64
'''
```

Dataframe中每一列是一个 Series, 但是 Series 没有列名. 因为只有 Dataframe 才有列的概念
```python
df['OLD_POP']  # 如果取出一列，返回 Series
```

将series变为list
```python
sr.tolist()
```

### 过滤 DF
```python
# 取出df中的一列
df.loc[:, ['OLD_POP']] 

# 去掉 ID 列
df = df.loc[:, df.columns != 'ID'] 

# 取出部分列
df = df[["Device Name", "Product Name", "Product Version"]] 

# 使用 == 来过滤
mask = user_input_df['Peer Device Role'] == 'OLD_POP' # mask是所有过滤出来的行   
user_input_df.loc[mask, 'Source'] = 'remove_old_pop' # 对过滤出来的行来赋值

# 用 endswith
df["col"].str.lower().str.endswith('word', na=False) # 如果单元格为空返回False

# 用 contains, 本质上是调用了正则中是search
df[df["col"].str.contains("ball", regex=True, na=False)]
# 可以使用正则
df[df['col'].str.match(r'^P.*') == True]

# 使用query来过滤， 速度比 == 快一倍， 但是需要写查询语句
user_input_df.query('`Peer Device Role`=="OLD_POP"') 

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

### 对列赋值
对一列赋值常数，需要先copy原来的df, 否则会报告警
```python
df_copy = df.copy()
df_copy.loc[:, 'col1'] = 'value'

```

### 运算

apply最慢， 因为是调用python语法逐行计算的。 向量化的方法都比较快，因为是调用c 代码执行的
```python
# 将计算后的列插入df
# 这里函数 get_new_tunnel_id 也是一个df
te_tunnel_df['new_tunnel_id'] = te_tunnel_df['Tunnel ID'].apply(get_new_tunnel_id)
```

```python
# Python 3.6.5, NumPy 1.14.3, Pandas 0.23.0

np.random.seed(0)
N = 10**5

# map的参数时Series
%timeit list(map(divide, df['A'], df['B']))                                   # 43.9 ms

%timeit np.vectorize(divide)(df['A'], df['B'])                                # 48.1 ms
%timeit [divide(a, b) for a, b in zip(df['A'], df['B'])]                      # 49.4 ms
%timeit [divide(a, b) for a, b in df[['A', 'B']].itertuples(index=False)]     # 112 ms
%timeit df.apply(lambda row: divide(*row), axis=1, raw=True)                  # 760 ms
%timeit df.apply(lambda row: divide(row['A'], row['B']), axis=1)              # 4.83 s
%timeit [divide(row['A'], row['B']) for _, row in df[['A', 'B']].iterrows()]  # 11.6 s

```

复制一行加在末尾
```python

row_copy = df.iloc[-1].copy()  # 复制最后一行为Series， 这种Series是带index的
row_copy['Source'] = 'undo_path' # 修改里面一个字段
df = df.append(row_copy)  # 把修改后的行添加到原来df的最后
```

# Indexing, Selecting, Assigning
```python
# 取一列
df.['col_name']
df.col_name

# 取一个值, index其实是行号
df.['col_name']['index']

### index-based selection 只能放列的编号, 不能放列名
# 取出第一行
df.iloc[0]
# 定位一个df中第一行的某个元素
df.iloc[0]['colname']  # 这里0是行数
df.colname[0]
df.colname.loc[0]
# 选择第0 列, 所有的行. 也就是第一列所有元素
df.iloc[:, 0]
# 选择第0 列, 0-2行
df.iloc[:3, 0]
# 选择指定行
df.iloc[[0, 1, 2], 0]
# 选择倒数 5 行, 所有列
df.iloc[-5:]

### Label-based selection 可以放列名
# 选择第 0 行, country 列
df.loc[0, 'country']
# 选中所有行, 指定列
df.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
```

注意: 在 loc 中的 0:10 其中 10 是包含能选到的
iloc 中的 0:10 其中 10 是选不到的

### Condition

```python
reviews.country == 'Italy'
"""
0          True
1         False
          ...  
129969    False
129970    False
Name: country, Length: 129971, dtype: bool
"""
```


```python
# 查找单个元素
df.loc[df['column_name'] == some_value]  

# 多个条件
df.loc[(df['column_name'] >= A) & (df['column_name'] <= B)]
df.loc[(df['column_name'] >= A) | (df['column_name'] <= B)]
# 在一个序列里
df.loc[reviews.country.isin(['Italy', 'France'])]

# 非空的行
df.loc[reviews.price.notnull()]
# 判断df是否数据为空
df.empty

```

对列赋值
```python
# 每一行的值都是 everyone
reviews['critic'] = 'everyone'
```

```python

# 数字类型
reviews.points.describe()
"""
count    129971.000000
mean         88.447138
             ...      
75%          91.000000
max         100.000000
Name: points, Length: 8, dtype: float64
"""
# 取和
reviews.points.sum()
# 取平均
reviews.points.mean()
# 取中位数
median_points = reviews.points.median()
# 去重
countries = reviews.country.unique()
# 列中内容重复重现的次数, 类似与 groupby 的统计
reviews.taster_name.value_counts()
# 最大值的 index, 对 NaN 过滤了
bargain_idx = (reviews.points / reviews.price).idmax()
bargain_wine = reviews.loc[bargain_idx, 'title']

# mapping 
# map是对 Series 的函数, 对一列中的每一个元素进行操作
review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
n_fruity = reviews.description.map(lambda desc: "fruity" in desc).sum()

# apply 是对 DF 操作的函数
# apply 使用的函数的入参是行, 可以返回行
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

# 也可以返回数值
def rate_star(row):
    if row.country == 'Canada':
        return 3
    if row.points >= 95:
        return 3
    if row.points >= 85:
        return 2
    return 1
# 对列进行操作
reviews.apply(func, axis='columns') # 或者 axis=1
# 对行进行操作
reviews.apply(func, axis='index') # 或者 axis=0
# 字符类型
reviews.taster_name.describe()
"""
count         103727
unique            19
top       Roger Voss
freq           25514
Name: taster_name, dtype: object
"""

```

map 和 apply 的区别
map 会快, apply 要慢
apply 只能对 DF 用
map 可以对 DF, Series
- Use **map()** when transforming a **single column/Series** element-wise.
- Use **apply()** when you need **row/column-wise** operations or DataFrame-level logic.
### 列操作
```python
# 列数字操作
review_points_mean = reviews.points.mean()
reviews.points - review_points_mean

# 列的字符串拼接
reviews.country + " - " + reviews.region_1
```

### Group by

- `count()`只和 `groupby()`连用. 如果只是列的技术用 `value_counts()`
```python
# groupby 之后 呈现 points 的计数
reviews.groupby('points').points.count()
# groupby 之后, 只要计数, 不用显示某一列的数据
reviews.groupby('taster_twitter_handle').size()

```

对两列进行 groupby
```python
# 可以对两列进行 groupby
# 取 points 的最大值
reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])

```

![[Pasted image 20241013060619.png]]


```python
reviews.groupby(['country']).price.agg(["len", "min", "max"])

```

![[Pasted image 20241013060712.png]]

Multi-indexes
```python
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
countries_reviewed
```


![[Pasted image 20241013060757.png]]

```python
countries_reviewed.reset_index(

```
![[Pasted image 20241013060824.png]]

```python
countries_reviewed = countries_reviewed.reset_index()

```

### Sorting index
```python
countries_reviewed.sort_values(by='len')

countries_reviewed.sort_values(by='len', ascending=False)

countries_reviewed.sort_index()
countries_reviewed.sort_values(by=['country', 'len']
# 把 price 作为 index, 也是分组的对象. 对标的数值是 points且已经每个分组找到 max
# 最后对 price 这个 index 排序
best_rating_per_price = reviews.groupby('price').points.max().sort_index()
```

# Data Types

```python
# show all columns data type
reviews.dtypes
# show one column data type
reviews.price.dtype
# convert data type
reviews.points.astype('float64')

```

# Fill NULL
- fill null的操作返回是Series
- fil null的操作不会影响原表的列
```python
reviews[reviews.price.isnull()]
reviews[pd.isnull(reviews.country)]

# count 空值
len(reviews[reviews.price.isnull()])
reviews.price.isnull().sum()

# fill
reviews.region_2.fillna("Unknown")
# replace
reviews.taster_twitter_handle.replace("@kerinokeefe", "@kerino")
```

# 操作 DF
### Rename
```python
reviews.rename(columns={'points': 'score'})
reviews.rename(index={0: 'firstEntry', 1: 'secondEntry'})
```


```python
reviews.rename_axis("wines", axis='rows').rename_axis("fields", axis='columns')

```
![[Pasted image 20241013075821.png]]
### Delete column

```python
# 删除 id列
df = df.drop(columns=["id"])
 ```
### Combining
```python
# combine two dataframes, ignore index， 两个dataframe结构一致
# 纵向叠加两个表
df_interface = pd.concat([df1, df2], ignore_index=True)

# join 关联两个表
pd.merge(df1, df2, on='business_id', how='outer')

```

join 的时候, 确保两个表的 index 是对应的
```python
left_df = canadian_youtube.set_index(['title', 'trending_date'])
right_df = british_youtube.set_index(['title', 'trending_date'])

left_df.join(right_df, lsuffix='_CAN', rsuffix='_UK')
```

# 统计

```python
df['field'].mean()  # 平均值

df.groupby('Year')['Height'].mean()  # group by
```


# 构建新的DF

### explode
如果某一列的元素是一个列表， explode可以对这一列做叉积

```python

df = pd.DataFrame({ 'card': ['1971 Nolan Ryan', '1928 Ogden Don Bradman', '1909 T206 Ty Cobb', '1887 Lone Jack Ben Franklin', '2005 Topps Justin Verlander'], 'properties': [['Baseball', 'Vintage', 'Pitcher'], ['Cricket', 'Pre War'], ['Baseball', 'Pre War', 'Batter'], ['Non Sports', 'Pre War'], ['Baseball', 'Modern', 'Pitcher']] })

# 可以发现 properties里面保存的是一个列表
df = df.explode('properties')  # 会把properties中每一行的展开为多行
```

# 读取sqlite

```python
import pandas as pd

engine = create_engine(f'sqlite:///{DB_FILE}')
table_df = pd.read_sql_table(  
	'license_item',  # table name
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

把df转为字典
```python
# 每一行为一个字典， 整体为一个序列
df.to_dict('records')

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

# 压制warning

```python
import warnings

with warnings.catch_warnings(record=True):  
	warnings.simplefilter("always")  
	offboard_df = pd.read_excel(file_path, sheet_name='sheet', engine="openpyxl")
```


