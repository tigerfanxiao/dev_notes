
在 http 中只能传递文本, 那网页html 中的图片, pdf 这些二进制的数据, 都会被 base64 编码后用明文的方式才传输, 在客户收到后, 在进行解码

Base64的常见使用场景
- 网页上的二进制数据传输
- 邮件中的内容也用 base64 编码

```python
import base64

def bytes_b64(convert_bytes):
	bytes_b64code = base64.b64encode(convert_bytes)
	return bytes_b64code.decode()

def b64_bytes(b64):
	b4code_back = bytes(b64, 'utf-8')
	signature = base64.b64decode(b4code_back)
	return signature
```


# URL 编码

urldecoder.org 就是在 url 里的特殊字符, 会进行编码

```
在 google 中搜索 返校, 会出现如下的 url 编码
https://www.google.com/search?q=%E8%BF%94%E6%A0%A1
```
