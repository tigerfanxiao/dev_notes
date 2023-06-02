安装包依赖
```
pip install pyyaml
```

读出来的可以直接是列表， 或者字典

```yaml
- 1
- 2
- 3
```

```python
import yaml

# 读取文件
with open(os.path.join(TEMP_DIR, path), 'r') as f:
	content = yaml.safe_load(f)

# 存储到文件
with open(temp_pwid_path, 'w', encoding='UTF-8') as f:  
	yaml.dump(self.result, f)
```