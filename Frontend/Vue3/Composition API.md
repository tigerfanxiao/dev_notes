
我们之前使用的是Option API

```javascript
export default {
  data() {
	  return {
		  bar: 'foo',
	  }
  },
  methods: {
	  greeting() {
		  return 'Hello World'
	  }
  }
}
```

composition
```javascript
export default {
	setup() {
		const bar = ref('foo')
		function greeting() {
			return 'hello world!';
		}
		return {
			bar, 
			greeting,
		}
	}
}
```



