区分animation和transition， 好像animation用于移动， transition用于色彩和属性的变化，有待考察
但是animation和transition duration冲突的时候，vue可以在transition的标签中设置type属性， 来告诉vue优先使用那个duration

还有问题是， animation在页面加载的时候，可以已经执行完毕了。 所以增加appear属性使用懒加载来实现。也就是说等到页面加载完来在做animation

vue为animation提供了6个hook
before-enter 
enter
after-enter
before-leave
leave
after-leave
# toggle一个组件

## 方法1
使用v-if

# 控制两个组件轮流出现

## 方法1
使用v-show
## 方法1
使用v-if
* 使用mode
默认情况下， 在transition标签中的两个组件，区分entering和leave两类
默认mode = in-out
在in-out模式下： entering in 的组件先进入，leave out的组件再出去
如果改为out-in模式： leave out的组件先out， entering的组件再进去
* 使用key

```vue
<template>
  <button type="button" @click="flag=!flag">Toggle</button>
  <transition name="fade" mode="out-in">
    <h2 v-if="flag" key="main">Hello World!</h2> // key 为了让vue分辨两个组件
    <h2 v-else key="another">Another Hellow world</h2>
  </transition>
</template>

<script>
export default {
  name: "App",
  data(){
    return {
      flag: false
    }
  }
}
</script>

<style>
.fade-enter-from {
  opacity: 0;
}
.fade-enter-active {
  transition: all 0.25s linear;
}

.fade-leave-to {
  transition: all .25s linear;
  opacity: 0;

}

</style>
```


vue 优先使用定义在transition标签中的duration， 之后才是css中定义的duration

```vue
  <transition name="fade" mode="out-in" duration="1000"> 
    <h2 v-if="flag" key="main">Hello World!</h2>
    <h2 v-else key="another">Another Hellow world</h2>
  </transition>
```


Javascript
event
bore-enter(el)
enter(el, done)
after-enter(el)
before-leave(el)
leave(el, leave)
after-leave(el)
enter-cancelled(el)
leave-cancelled(el)

一般情况下，优先使用css animation，因为占用的资源比较少。 在同时使用css和js的情况下， vue会优先参考css的duration， 所以在js的代码中，对于enter和leave，可以不用调用done方法

transition component is only for one element
