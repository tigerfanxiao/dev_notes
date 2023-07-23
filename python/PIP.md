```shell

pip search <PACKAGE_NAME>  
pip install <PACKAGE_NAME>==1.0.0
pip install ––upgrade <PACKAGE_NAME
pip list  # show all installed package

```


### requirements.txt
```shell

pip install –r <REQUIREMENTS_FILE>

pip freeze > reqirements.txt
```

通过华为内部镜像来安装 pip 包
```shell
python -m pip install treelib==1.5.5 -i http://mirrors.tools.huawei.com/pypi/simple/ --trusted-host mirrors.tools.huawei.com --user

```


# 通过 git 来安装 python 包
这里要注意, 如果从 github 上下载包, 可能有些包在 beta 或者测试阶段, 所以要先确保下载包的版本是稳定的生产版本
```shell
# git clone netmiko
git clone https://github.com/ktbyers/netmiko.git netmiko

# 
cd netmiko
ls -al # 找到 setup.py
python setup.py install # 安装
```