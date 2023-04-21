
用文字的方式动态导入包
```python
import importlib

# if you have a python pakage design_file
# - design_file
#  |- type1.py
#  |- type2.py
type2 = importlib.import_module('design_file.type2')

# if you want a specific funtion
func1 = getattr(tye2, 'func1')
```



