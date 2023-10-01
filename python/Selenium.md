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
from selenium.webdriver.chrome.service import Service
  
options = Options()  
options.page_load_strategy = 'normal'  
  
PATH = "./chromedriver.exe"  
  
driver = webdriver.Chrome(  
    service=Service(PATH),  
    options=options,  
)  
  
  
driver.get("https://www.google.com")  
  
  
driver.quit()  # 退出浏览器
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
el_username = WebDriverWait(driver, 20).until(EC.element_to_be_clickable((By.ID, "username")))
el_username.send_keys("your name")  # 发送数据

```

# 键盘输入

```python
from selenium.webdriver.common.keys import Keys

el_username.send_keys(Keys.RETURN)
```

# 跳转登录
有的网页使用cookies登录， 有的网页使用token登录
## 获取cookie
```python
driver.get_cookies()  # 获取所有cookies, 一个列表
driver.get_cookie(name)  # 根据cookie的name来获取指定cookie

# 添加cookie
driver.add_cookie({'name': '...', 'value': '...'})
```

## 使用token

```python
token = driver.execute_script('window.localStorage.getItem("token")')
# 添加token
driver.execute_script('window.localStorage.setItem("token","token值")' )

browser.refresh() # 刷新浏览器
```

## 隐藏浏览器

```python
options.headless = True 
```