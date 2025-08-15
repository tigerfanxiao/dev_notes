

如果通过可以练习提升自己的 coding style? 
-   什么是 mysql 中的行锁, 来保证原子操作, 如何实现?

  

-   TopK 问题的解决方法
-   什么是跳跃表 Skip-list, 它如何灵活地处理插入和读取 log(n), 可能是链表的变形

  

-   有什么办法用 mysql 来代替 ip 地址

这个 comment_set 是怎么关联上的, 通过 comment表里查询 tweet_id?

comments = CommentSerializer(source='comment_set', many=True)

提问: 怎么优化获取所有 comment 提高效率

-   __all__ 启动 celery 不太懂
-   celery 的 worker 和 server 用的是相同的代码?

那用户发送请求到 server 后, server 是自动把传给 MQ, MQ 再分配给 worker? 还是说 work 知道 celery 的 MQ 的 ip 地址, 去取任务?
  

**自己查询的问题**

-   什么是 topK 问题

[https://zhuanlan.zhihu.com/p/76734219](https://zhuanlan.zhihu.com/p/76734219)

-   mapreduce 只能处理离线任务, 不能处理实时任务
-   什么是GeoHash
-   什么是 KMP 算法
-   查看一下5xx 报错收集工具 sentry 的用法


**回答了的问题**

-   make build, make up, make log, 每次都会新建一个 docker 吗

-   不会新建 docker
-   项目用的前端框架是什么？应该是 react. 有另外的课程项目