在线检查语法工具
https://onlineyamltools.com/

# YAML 语法

### 转义
- 因为 ansible 已经是用 `:[]{}>` 如果要使用这些字符原来的意义, 需要转义
- YAML 支持直接读取 True 和 False. 如果要读成字符串, 需要双引号
- YAML 支持直接读取数字. 如果要读成字符串, 需要双引号


### YAML 特性
* 在一个 yaml 文件中可以放多段配置, 并用分隔符分开
* YAML也支持注释. 不区分单双引号
```yaml
---  # 只是 document seperator
inventory 
```
#### 换行

- 一行写成多行, 不保留格式. 常用语在一行内容太多的时候
- 保留格式显示多行. 常用语直接写 bash 脚本时
```yaml 
- template: >
	src/srv/httpd.js
	dest=/etc/httpd.conf
# 上面一行等于 
# - template: src/srv/httpd.js dest=/etc/httpd.conf

command:
	- sh
	- -c
	- |
		#!/usr/bin/env bash -e 
		https () {
			local path "${1}"
		}
		http "/app/kibana"
```
#### 列表和字典
```yaml
---
inventory:
  device:
    - name: csr1kv1
      version: 16.09
      vendor: cisco
      uptime: '2 days'  # 有空格字符串用引号包起来
      serial: XB96871
      snmp:
        - name: public
          permission: ro
        - name: private
          permission: rw
        - version: [1.2, 1.0] # list 的另外一种方式

```

# Python with Yaml
python 的 yaml 包
```
pip install pyyaml
```

```python
import yaml

# 读取文件
with open(os.path.join(TEMP_DIR, path), 'r') as f:
	content = yaml.safe_load(f)  # content 是一个字典

# 存储到文件
with open(temp_pwid_path, 'w', encoding='UTF-8') as f:  
	yaml.dump(self.result, f)
```