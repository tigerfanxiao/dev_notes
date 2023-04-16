要解压文件
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