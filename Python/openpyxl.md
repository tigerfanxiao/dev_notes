
### 行列计数

* 必须要要用cell方法
* 行列技术都从1开始

### 读出表格中的内容，而不是公式
```python
from openpyxl import load_workbook

# 读出单元格中的内容，而不是公式
wb = load_workbook(file_path, data_only=True)
```


### workbook 属性
```python
wb.active # 当前活动sheet
wb.sheetnames # 所有的sheet
```

### Sheet 属性
```python
# 表格最后一个非空行
ws.max_row
# 当前活动页
ws.max_col

# 控制列宽
col_id = 1
ws.column_dimensions[string.ascii_uppercase[col_id]].width = 30

```

### 创建新的文件
```python
# 对默认的sheet1 进行重命名
wb = openpyxl.Workbook()
ws = wb['Sheet']
ws.title = 'Fruit'


# 删除sheet
default_sheet = wb.get_sheet_by_name('Sheet')
wb.remove_sheet(default_sheet)
```

创建出了默认sheet之外的文件
```python
wb = openpyxl.Workbook()
wb.create_sheet('sheet name')  # create new sheet

# 操作cell只能用cell方法
ws.cell(row=2, column=2).value = 2  # save value to cell

wb.save(filepath)

```


### 单元格

空单元格的value属性是None
```python
ws.cell(row=2, column=2).value is None

```


### 下拉框 Data Validation

```python
import openpyxl
from openpyxl.worksheet.datavalidation import DataValidation

wb = openpyxl.load_workbook(TEST_EXCEL)  
sheet = wb.active  
  
valid_options = '"Not started, In Progress, Completed"'  
rule = DataValidation(type='list', formula1=valid_options, allow_blank=True) 

rule.error = 'Your entry is not valid'  
rule.errorTitle = 'Invalid Entry'  
  
rule.prompt = 'Please select from the list.'  
rule.promptTile = 'select Option'  

sheet.add_data_validation(rule)  
rule.add('c2:c100')  # 在指定的区域设置数据验证
wb.save('file.xlsx')   # 修改后的文件另存为新的文件

```