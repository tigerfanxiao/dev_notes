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
}).mount("#app")

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

* 鼠标点击事件做好都prevent掉

```html

<input type="text" :value="lastName" @input.prevent="updateLastName('private messag', $event)"/>
```

# key modifer

感悟： 事件的值可以写自定义的函数名， 也可以写变量的公式
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

### computed properties

下面这种表达式就是计算表达式
这种表达式的问题就是，只要任何data里面的数值变动，都会触发computed properties的重新计算。 
当我们把计算式放在computed property中后， 我们可以把它当作一个变量

```html
<!-- 这里不再需要括号 -->
<p>{{ fullName }}</p>
```

```javascript
 const vm = Vue.createApp({
	 data() {
	 }, 
	 methods: {
		  
	 }, 
	 computed: {
		fullName() {
			 return `${this.firstName} ${this.middleName}`
		},
	 }
	 
 }).mount("#app")
 

```
### 区分method property 和 computed property
* computed property 提高了performance， 它只会对函数设计的变量在变化的时候，做重新计算
* computed property主要用于计算数值，所以每个函数都需要有返回值
* method property可以没有返回值，可以用来做业务，或者发送请求
* 在computed property中， vue只是保存计算后的数值，而不是函数本身。而method property中保存的是函数
* 因为我们把computed property任务是数值或者变量，我们也不能对函数传入参数