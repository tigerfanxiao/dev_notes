
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