
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



# 工具
### Eslint
eslint不是vite的包
```shell
npm install eslint --save-dev
# 默认情况下vite不包含eslint，下面这个包强制vite在运行js程序前需要通过eslint检查
npm install vite-plugin-eslint --save-dev --force

```

在项目的根目录，创建 `vite.config.js`
```javascript
import { defineConfig } from 'vite' // 这个插件是帮助编辑器提示的配置方式
import eslint from 'vite-plugin-eslint'

export default defineConfig ({
	plugins: [eslint()]
})
```

eslint本身还需要一个配置文件`.eslintrc`

```json
{
	"rules": {
		"quotes": "error"  // off， warn, error, 强制要双引号
	}, 
	"env": {
		"browser": true
	}, 
	"parserOptions": {
		"ecmaVersion": 2022, // version of JS
		"sourceType": "module"  // 控制import statement
	}
}
```

但是这个只是在运行开发环境的时候，在console报错， 如果要从代码的角度看到错误的地方，还是需要安装vscode的eslint插件
我们可以手动修复这些问题，也可以让eslint来修复
在`package.json`文件中
```javascript
"scripts": {
	"lint": "eslint main.js --fix"
}
```

配置完成之后，重启开发环境
