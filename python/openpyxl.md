### 行列计数
单元格的行和列是从1开始计数的
* 列计数从0开始

### 读出表格中的内容，而不是公式
```python
from openpyxl import load_workbook

# 读出单元格中的内容，而不是公式
wb = load_workbook(file_path, data_only=True)

```



### 创建新的文件
```python
# 对默认的sheet1 进行重命名
wb = openpyxl.Workbook()
ws = wb['Sheet']
ws.title = 'Fruit'
```

创建出了默认sheet之外的文件
```python
wb = openpyxl.Workbook()
wb.create_sheet('sheet name')  # create new sheet

ws.cell(row=2, column=2).value = 2  # save value to cell

wb.save(filepath)

```

