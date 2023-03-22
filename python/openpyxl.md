
### 读出表格中的内容，而不是公式
```python
from openpyxl import load_workbook

# 读出单元格中的内容，而不是公式
wb = load_workbook(file_path, data_only=True)
```

