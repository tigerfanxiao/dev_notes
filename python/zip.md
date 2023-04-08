
```python
import zipfile

def unzip_hmo_log_files(zip_file, output_dir):  
    device_log_path_list = []  
    with zipfile.ZipFile(zip_file, mode='r') as archive:  
        for file in archive.namelist():  
			archive.extract(file, output_dir)  
            device_log_path_list.append(os.path.join(output_dir, file))  
    return device_log_path_list  # 返回的是加压后的文件路径列表

```