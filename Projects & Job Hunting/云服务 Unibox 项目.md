# 项目背景

1.  介入Local business 数据, 模仿 google merchant center

# 项目方案

1.  Category 层级结构还是水平结构的讨论, 使用多标签模式, 主标签
2.  jwt token 鉴权方式
3.  HUAWEI Cloud 服务, OBS 接受数据, 文件格式, 命名, 标准的定义
4.  working hour 字段定义, 对于国定假使用时间段的切割方式, 还是使用正段时间扣除休假方式
    前一种方案在代码上容易实现, 后一种方式对于CP来说更容易处理
5.  areaService 引入GeoJSON 提出服务区域概念 (polygon, circle, point) 和 petal map合作
6.  推动 UTM 值(Campaign 项目)引入, 流量追踪, 优惠分成, 流量分成方案
7.  推动轮训通知功能
8.  新闻分类 fliter 功能, 运用机器学习,区分问题, interview, report, real-time news, 媒体来源的权重
9.  Shopping product ranking, 对于中小CP是否提高曝光率, 权重.
10.  推动API的多语言支持

# 项目工作

1.  与CP workshop, 介绍解决方案
2.  对于API测试到商用的测试, 联调使用文档编写. Postman collection 测试用例编写
3.  领导4人团队, 面对客户的 Flight API 进行开发. Python request
    解决的问题: latency, cache 20秒数据,
4.  寻取多个CP API共性设计, 字段映射自动化工具, 通过配置文件完成对接, 而不是每次多独立开发.
    这些经验, 将用于设计未来的HUAWEI Flight API

