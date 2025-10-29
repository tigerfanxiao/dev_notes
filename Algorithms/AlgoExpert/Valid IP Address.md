# 题目
给定一个字符串 `19216812`, 我们要把他们拆成可能的ip地址。 比如
```
1.92.168.12
19.2.168.12
19.21.68.12
19.216.8.12
19.216.81.2
192.1.68.12
192.16.8.12
192.16.81.2
192.168.1.2
```

# 分析
1. 这道题的关键是用什么方法将字符传拆开为4段， 且每一个阶段都符合ip地址分段的约束。
2. 考虑第一段ip地址， 可以对字符传进行分片。 第一段可以包含1，2，3个元素， 对应的第二段在第一段的基础上， 可以包含 1，2，3 个元素。 就这两端来看， 就是一个嵌套循环。 
3. 考虑验证每个分段的有效性. 比如一个分段不能大于 255, 不能以 0 开头

```python
# time： O(1)
# space: O(1)

def validate_ip(string):  
	ip_address_found = []  
	current_ip_address_parts = ["", "", "", ""]  
	# min表示如果字符串短于4, 就应该字符串本身的长度。 
	for i in range(1, min(len(string) - 2, 4)):  
		current_ip_address_parts[0] = string[:i]  
		if not is_valid_part(current_ip_address_parts[0]):  
			continue  
  
		for j in range(i + 1, i + min(len(string) - i, 4)):  
			current_ip_address_parts[1] = string[i:j]  
			if not is_valid_part(current_ip_address_parts[1]):  
				continue  
  
			for k in range(j + 1, j + min(len(string) - j, 4)):  
				current_ip_address_parts[2] = string[j: k]  
				current_ip_address_parts[3] = string[k:]  
				if all(map(is_valid_part, current_ip_address_parts[2:])):  
					ip_address_found.append(".".join(current_ip_address_parts)) 
	return ip_address_found
  
# one helper function
def is_valid_part(part):  
	part_as_int = int(part)  
	if part_as_int > 255:   # 每个分段都不大于255
		return False  
	return len(part) == len(str(part_as_int)) # 这里确保每个分段不会有0开始 no leading 0
```