读出来的可以直接是列表， 或者字典

```yaml
- 1
- 2
- 3
```

```python
import yaml

with open(os.path.join(TEMP_DIR, path), 'r') as f:
	content = yaml.safe_load(f)
```