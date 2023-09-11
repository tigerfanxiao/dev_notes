实现了 Infrastructure as Code 通过模板来构建基础设施. 而所谓的模板是文本文件
- 用 yaml 或者 json 来管理配置
- 用 git 来记录配置变动的版本
- 支持模板嵌套

最佳实践
1. 快速构建测试床, 在测试完成后, 又可以马上删除, 来节约资源
2. 快速在其他 region 构建备份站点



DeletionPolicy 控制用户不能随便删除资源

在创建资源前先创建 stack
把配置保存在文件中
创建 S3 Bucket
```json
{
    "Resources": {
        "catpics": {
            "Type": "AWS::S3::Bucket"
        }
    }
}
```