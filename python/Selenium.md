# 安装

1. python package
```shell
pip install selenium
```
2. 查询当前google chrome的核心， 需要下载匹配的browser driver
	[Chrome Driver](https://chromedriver.chromium.org/downloads)


```python
from selenium import webdriver  
from selenium.webdriver.chrome.options import Options  
  
  
options = Options()  
options.page_load_strategy = 'normal'  
  
  
PATH = "./chromedriver.exe"  
  
driver = webdriver.Chrome(  
    executable_path=PATH,  
    options=options,  
)  
  
  
driver.get("https://www.google.com")  
  
  
# driver.quit()
```

### 满足条件EC
```python
from selenium import webdriver  
from selenium.webdriver.chrome.options import Options  
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC

options = Options()  
options.page_load_strategy = 'normal'  
 
PATH = "./chromedriver.exe"    
driver = webdriver.Chrome(  
    executable_path=PATH,  
    options=options,  
)
# 条件满足后，元素出现了，可以点击了
WebDriverWait(driver, 20).until(EC.element_to_be_clickable((By.ID, "username"))).send_keys("xwx1087747")
```

