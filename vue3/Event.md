
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