# 通过event来传递数据
事件有两种写法
* v-model
* v-bind:value + @

### v-model 写法
更短
```html
<body>
	<div id="app" v-cloak>
		<label>Last Name</label>
		<input type="text" v-model="lastName" />
	</div>
	<script src="https://unpkg.com/vue@3"></script>
	<script src="app.js"></script>
</body>


```

```javascript
// app.js
const vm = Vue.createApp({
	data() {
		return {
			lastName: 'fan'
		}
	},
})

```


### v-bind写法
需要添加一个input事件
```html

<body>
	<div id="app" v-cloak>
		<label>Last Name</label>
		<input type="text" :value="lastName" @input="updateLastName"/>
	</div>
	<script src="https://unpkg.com/vue@3"></script>
	<script src="app.js"></script>
</body>

```

```javascript
// app.js
const vm = Vue.createApp({
	data() {
		return {
			lastName: 'fan'
		}
	},
	methods: {
		updateLastName(event) {
			this.lastName = event.target.value
		}
	}
})

```

# 向event传递数据

让event的函数接受变量

```html

<body>
	<div id="app" v-cloak>
		<label>Last Name</label>
		<input type="text" :value="lastName" @input="updateLastName('private messag', $event)"/>
	</div>
	<script src="https://unpkg.com/vue@3"></script>
	<script src="app.js"></script>
</body>

```

```javascript
// app.js
const vm = Vue.createApp({
	data() {
		return {
			lastName: 'fan'
		}
	},
	methods: {
		updateLastName(msg, event) {
			// 这里可以用event.modifier改写
			event.preventDefault()
			console.log(msg)
			this.lastName = event.target.value
		}
	}
})

```

# event.modifier

Vue 提供了多种event.modifer


```html

<input type="text" :value="lastName" @input.prevent="updateLastName('private messag', $event)"/>
```

# key modifer


```javascript
// 回车触发事件
<input type="text" @keyup.enter="updateMiddleName"/>

// 按下ctrl 点击触发事件
<button type="button" @click.ctrl="age--">Decrement</button>```

```

### model modifier
因为input标签默认读取的信息是字符串， 所以再model.modifier进行转换成number

```javascript
<input type="text" v-model.number="age"/>
```

输入懒加载
lazy只有再focus依赖input按钮时才会加载
trim不但去掉字符串前后的空格， 字符串中间的多个空格也只会保留一个
```javascript
<input type="text" v-model.lazy.trim="firstName"/>
```