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