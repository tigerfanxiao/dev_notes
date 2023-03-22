### 规避SSL Error
```python

import requests  
  
r = requests.get('https://netlive-de.gts.huawei.com/fbb', verify=False)  
print(r.url)

```