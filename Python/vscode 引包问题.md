在vscode中时，常常跨包引入module的问题
* 首先创建一个虚拟环境，否则本地的包会安装到全局的环境中
* 在项目的root目录下， 创建setup.py
```python
# setup.py
from setuptools import setup, find_packages

setup(name='MyPackageName', version='1.0.0', packages=find_packages())
```

* 在项目的root目录下，运行. 把目录下的包都安装了
```shell
pipenv install -e .
```
