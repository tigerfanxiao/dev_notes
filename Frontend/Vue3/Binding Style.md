
数值 + px的情况
```html
<div class="circle" :style="{ width: size + 'px'}"></div>
```

如何使用css 属性
* 使用引号
```html
<div class="circle" :style="{ width: size + 'px', 'line-height': size + 'px'}"></div>
```
* 使用camel case

```html
<div class="circle" :style="{ width: size + 'px', lineHeight:size + 'px'}"></div>
```