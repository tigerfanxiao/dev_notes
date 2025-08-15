# 使能 Hyper-V
Powershell 管理员权限下执行 
```shell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

# 创建虚拟机

在 win11 上可能需要变更 hard disk目录`C:\Users\Public\Documents\Hyper-V\Virtual hard disks`

# 桥接模式

1. 在 Virtual Switch Manger 中创建一个 External Switch 命名为 Bridge Switch
2. 修改虚拟机的 Setting 中 Network Adapter 从 default switch 切换为 Bridge Switch
