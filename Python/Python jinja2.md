


```python
import jinja2

env = jinja2.Environment(loader=jinja2.FileSystemLoader(TEMPLATE_DIR)))

template = env.get_template('csg_script_seamless.j2')
device_config = template.render(self.content)

with open(output_file_path, mode='w', encoding='utf-8') as f:  
	f.write(device_config)  
	
```