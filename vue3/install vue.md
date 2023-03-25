
cdn方式安装
```html
<script src="https://unpkg.com/vue@3"></script>

```

vue dev tool extension
configure it to read file 

在一个page里可以有多个vue instance

```javascript
// app.js
Vue.createApp({}).mount('#app')

```

vue支持多个instance， 只要做两个id就行


vue的proxy， 使你写程序更方便

```javascript
vm.$data.firstName

// 使用了proxy
vm.firstName
```

vue支援js表达式

```html
<div id="app1">

      {{ `${firstName} ${lastName.toUpperCase()}` }}

</div>
```


method 属性， 在里面可以定义业务，而不是写在html里面

Directive 一般是标记在html里面，等vue全部加载完毕后就会消失

在chrome上用 ctrl + f5, 不但刷新页面，还把清空了缓存

```css
/*css attribute*/
[v-cloak] {
  display: None;

}
```

vue is two-way data-binding
html 和 js 互相绑定
* 前面已经说明了如何从js把数据传给vue
* 通过v-model 可以把html的前端输入把数据传给vue

前面说的v-model是用在标签内的内容的。但是要多标签的属性做bind
就用 `v-bing:` 加在属性前面, 这里`v-bind`可以省略

```html
<p><a v-bind:href="url" target="_blank">Google</a></p>

<p><a :href="url变量名" target="_blank">Google</a></p>

```

# 引入raw html
XSS cross site scritping
默认情况下你不能再变量里设置raw_html， vue会检测出来，并不展示
如果一定要引入，需要用 `v-html="html变量名"` 

