VLAN 可以认为是一种虚拟化技术. 通过 vlan 我们可以把一个交换机分为多个交换机.
每个交互机之间在二层网络中内隔绝开. 

比如我们把交换机的接口 1-10 分给 vlan1, 把接口 11-20 分给 vlan2. 你可以任务一个 vlan 就是一台虚拟的交换机. 那么在这种情况下, vlan1 和 vlan2 是无法完成二层通信的. 他们不会收到对方的广播报文. 

怎么实现vlan1 和 vlan2 的分割呢? 我们使用打 tag 的方法. 
当交换机从接口 1 收到一个帧的时候, 他们会在这个帧前面加一个 tag, 向这样
`vlan1_tag| dmac | smac`
交换机只会把这个帧从加入了 vlan1 的那些接口发出去, 而不会从加入了 vlan2 的那些发出去. 同时, 因为 tag 只是在交换机内使用, 所以在发出去的时候, 还得把这个 tag 给剥离了. 这种打 tag 的方式也称为 dot1Q

上面提到的是一种非常简化的环境, 非常小型的网络. 一台交换机就把所有的设备连接起来了. 
但是现实的情况更复杂. 我们考虑一种情况. 我们有幢楼, 每幢楼各有一台交换机. 然后我们有两个部门: 部门 1 和部门 2. 他们在两幢楼都有办公人员. 我们允许他们部门内部的通信, 但是不允许部门间相互通信, 要怎么实现? 这个时候我们就要引入 Trunk 了

# Trunk
要让两幢楼的人相互通信, 我们就要将两台交换机连接起来. 但是要隔开两个部门的人, 我们就要用 vlan 打 tag 的方式, 把接口划分到各自的 vlan下. 同时, 因为对面楼也有自己部门的人, 我们就要把数据通过两个交换机的连线发送出去. 这种情况下, 我们就需要一种新的接口, 称为 trunk 口. 这种 trunk 口的连线可以同时允许 vlan1 和 vlan2 的数据通过. 等数据传到了对面的交换机之后, 再根据帧中的 tag 信息来决定去往那个对应 vlan. 基于这种想法, 因为 tag 信息在到达对面交换机时是必要的, 数据在离开本地交换机 trunk 口的时候, 就不做剥离 tag 的动作了. 
对应的, 我们把之前那种需要做剥离动作的接口称为 Access 接口. 

可以简单的总结, Access 接口一般接的是终端设备, 而 trunk 接口一般接的交换机.
