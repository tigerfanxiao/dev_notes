
安装镜像需要做两件事
1. 下载 gns3 的 appliance, 本质上是一个 json 文件, 在 gns3 官网marketplace 可以下载
2. 在 gns3 中 import appliance
3. 需要下载对应的镜像文件 qcow2. 这个得自己找了. 有的是需要收费的


# 安装 iou 镜像

安装 iou license

```shell
wget http://www.ipvanguish.com/download/CiscoIOUKeygen3f.py
python3 CiscoIOUKeygen3f.py

cat iourc.txt
# 把 license 复制到 gns3 的 setting IOS ON Unix内
```