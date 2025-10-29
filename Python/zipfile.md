### 解压文件到指定目录

一次性解压所有文件

```python
import zipfile
my_zip_file = zipfile.ZipFile(zip_file_path)
my_zip_file.extractall(output_dir)
my_zip_file.close()
```


逐个解压文件
```python
import zipfile

def unzip_files(zip_file, output_dir):  
    device_log_path_list = []  
    with zipfile.ZipFile(zip_file, mode='r') as archive:  
        for file in archive.namelist():  # 这里已经包含了 zip 文件中的所有目录
			archive.extract(file, output_dir)  # 将文件解压到 output_dir下
            device_log_path_list.append(os.path.join(output_dir, file))  
    return device_log_path_list  # 返回的是加压后的文件路径列表
```


### 压缩指定目录下的文件

```python
import pathlib
import zipfile

directory = pathlib.Path("source_dir/")

with zipfile.ZipFile("directory.zip", mode="w") as archive:
   for file_path in directory.iterdir():
		# 这里如果arcname不配置， 在压缩文件内会保留很深的数据结构
       archive.write(file_path, arcname=file_path.name)

# 打印zip下的目录
with zipfile.ZipFile("directory.zip", mode="r") as archive:
    archive.printdir()
```
