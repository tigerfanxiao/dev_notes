默认命名 `Dockerfile`

在 ubuntu镜像里安装 python
```dockerfile
FROM ubuntu:lastest
RUN apt update
RUN apt install -y python3

WORKDIR /usr/app.src
# 将本地的 python 文件复制到 workdir下
COPY print.py ./
# 运行python文件
CMD ["python3", "./print.py"]
```


### RUN vs CMD
* RUN 只在容器被创建时执行, 重启不会执行
* CMD 每次容器重启都会运行

python 环境
``` dockerfile
FROM python:3.9-slim-buster
COPY ./requirements.txt ./requirements.txt
COPY ./main.py ./main.py
RUN pip3 install -r requirements.txt
CMD ["python3", "./main.py"]
```
docker deamon 会自动读取当前目录下的 Dockerfile 文件, 然后创建一个image
```bash

docker build -t <image-name> .  
```
在通过 image 创建容器
```bash
docker run <image-name>
```

