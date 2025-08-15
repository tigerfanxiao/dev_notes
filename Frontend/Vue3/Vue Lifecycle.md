蓝色表示vue或做那些事情
绿色表示lifecycle hooks， 我们可以给这些hook加一些动作
这里的el值html对象， 是vue instance mount的目标

![[Pasted image 20230325163946.png]]


|hooks|状态|描述|
|---|---|---|
|beforeCreate|这个时候proxy还没有创建，data property和method也没有创建|不能使用proxy|
|created|这个时候data和method已经创建好了|可以使用proxy访问data|
|beforeMount|这个时候Template已经编译好了，但是还没有mount到页面上||
|mounted|vue instance已经关联上取去了
|beforeUpdate|在data change作用到template之前||
|updated|在data change作用到template之后||
 

