也成为发布订阅者模式

内容： 定义对象间的一种一对多的依赖关系。当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。观察者模式又称“发布-订阅”模式


# event pattern

使用场景
1. 如果有多种事件，都需要类似的原子操作，我们可以把这个事件认为一个event
2. 定义一个event 字典，数据结构是`{event: [fn1, fn2, ...]}`
3. 定义两种操作event字典的方法： 注册event， 和执行event中的方法列表
4. 新建操作listener，面向每一个event要做到的handler是怎样的，并提供一个setup_handler函数，将handler注册到对应的event中
5. 如果某一个事件发生了， 首先获得data，执行event中的方法列表