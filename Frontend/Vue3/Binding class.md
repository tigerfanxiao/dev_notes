
* 传条件
* 传值
* 传列表
# 传条件
# 单个class
```html

<label>
	<!--v-model 可以绑定checkbox中数值，选中为true，否则为false -->
      <input type="checkbox" v-model="isPurple"/> Purple
</label>
<!-- classname_to_add: data_property_name -->
<div class="circle" :class="{ purple: isPurple}">
```
# 多个class 

```html

<label>
	<!--v-model 可以绑定checkbox中数值，选中为true，否则为false -->
      <input type="checkbox" v-model="isPurple"/> Purple
</label>
<!-- classname_to_add: data_property_name -->
<div class="circle" :class="circle_classes">
```

```javascript
const vm = Vue.createApp({
	data(){
		return {
			isPurple: false
		}
	}, 
	computed: {
		circle_classes() {
			return {
				purple: this.isPurple, 
				otherClass: condition,  
			}
		}
	}
}).mount("#app")

```

# 传值



# 传序列

```html
<div class="circle" :class="[circle_classes, selectedColor]">
```