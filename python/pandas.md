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
# 查看有多少行
len(df.index)

```

### 合并两个 DF
```python
# combine two dataframes, ignore index， 两个dataframe结构一致
df_interface = pd.concat([df1, df2], ignore_index=True)
```

