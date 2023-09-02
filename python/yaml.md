# YAML 语法

### 转义
- 因为 ansible 已经是用 `:[]{}>` 如果要使用这些字符原来的意义, 需要转义
- YAML 支持直接读取 True 和 False. 如果要读成字符串, 需要双引号
- YAML 支持直接读取数字. 如果要读成字符串, 需要双引号
- 


用 `>` 来换行

```yaml
- name: write the apache config file
- template: src/srv/httpd.js dest=/etc/httpd.conf

# 上面一行可以写成
- template: >
	src/srv/httpd.js
	dest=/etc/httpd.conf
```

安装包依赖
```
pip install pyyaml
```

YAML 特性
* 在一个 yaml 文件中可以放多段配置, 并用分隔符分开
* YAML也支持注释. 不区分单双引号
```yaml
---  # 只是 document seperator
inventory 
```

读出来的可以直接是列表， 或者字典
列表
```yaml
---
- 1
- 2
- 3
```

字典
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

```

# python
```python
import yaml

# 读取文件
with open(os.path.join(TEMP_DIR, path), 'r') as f:
	content = yaml.safe_load(f)  # content 是一个字典

# 存储到文件
with open(temp_pwid_path, 'w', encoding='UTF-8') as f:  
	yaml.dump(self.result, f)
```