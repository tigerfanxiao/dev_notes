
这是一个创建vue项目的工具
```shell
# 创建vue 3 项目
npm init vue@3

# 创建 vue2 项目
npm init vue@2

```


alias
```javascript
// without alias
import Example from 'src/emaple'

// with alias
import Example from '@/example'
```


创建app component
为了查看.vue文件，安装 vscode volar插件
App.vue
```vue
<template>
  <p>{{ msg }}</p>
</template>

<script>
export default {
  name: 'App'  // development tool use to name the app
  data(){
	  return {
		  msg: "Hello World"
	  }
  }
}
</script>
```


创建子component
在src文件夹中建立一个components文件夹
在`src/components/Greeting.vue`
安装vscode plugin vue vscode snippets 来创建子component

有两种方法mount component： global， local
项目这个global注册方法有问题，没实现

```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import Greeting from '@/components/Greeting.vue'  // 引入子component

let vm = createApp(App)
vm.component("Greeting", Greeting)  // 使用component方法
vm.mount('#app')
```

global register不推荐，会降低webpack打包效率

```vue
<template>
  <greeting></greeting>  <!--component标签-->
</template>

  

<script>
import Greeting from '@/components/Greeting.vue'

export default {
  name: 'App',
  components: {
    Greeting  // es2015语法 = Greeting:Greeting
  }
}
</script>

<style scoped>


</style>

```