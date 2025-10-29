importlib 可以帮助我们动态引入包和函数, 所以我们在给函数命名的时候, 可以遵从一定的规律. 这样在使用策略模式的时候, 可以直接根据策略名来调用策略函数. 


用文字的方式动态导入包
```python
import importlib

# if you have a python pakage design_file
# - design_file
#  |- type1.py
#  |- type2.py
#  |- __init__.py 
type2 = importlib.import_module('design_file.type2')

# if you want a specific funtion
func1 = getattr(tye2, 'func1')
```



