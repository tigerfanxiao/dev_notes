
# 遍历序列
一般的for循环
```html
<li v-for="bird in birds" :clas="bird">{{ bird }}</li>
```

带有index的for循环

```html
<ul>
   <li v-for="(bird, index) in birds" :clas="bird">{{ bird }} {{ index }}</li>
</ul>
```

# 遍历对象序列

```html

<div v-for="(value, key, index) in person">
	{{ key }} {{ value }} {{ index }}
</div>
```

# key:
当操作序列中的元素时，vue本质上只是操作数据，而不是dom中的元素。本质上是为了性能考虑的。 如果要求vue连标签一起操作， 需要加上key

```html
 <div class="card" v-for="person in people" :key="person.name">
     <h3>{{ person.name }}</h3>
     <p>{{ person.message }}</p>
	  <input type="text">
</div>
```