
每次修改Lambda Function 相对应的ARN会变动. 可以给这个函数增加一个alias, 让alias指向修改后的Lambda函数. 其他系统在调用Lambda函数时, 只要调用Alias就行. 不用修改

# 属性

reserved concurrency limit 控制lamdda function并发数量