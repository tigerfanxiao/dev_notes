
SASS is programming language for css

SASS 会对css进行Compile

安装
```shell
npm install sass

```

如果已经安装了 sass, 
1. 把css文件改为scss文件
2. 把main.js中原来import css的命令改为import scss

# 使用SASS函数
```sass
h1 {
  color: #CC3244;
}
h1:hover {
	// 这里的darken是sass自定义的函数，接受两个参数，一般是5%到15%
  color: darken(#CC3244, 15%);
}
```

# 用SASS写选择器

```scss
h1 {
  color: #CC3244;
  // 等于 h1 span 指 h1内地span标签
  span {
    color: blue;
  }
  // = h1:hover 但是用&替代h1
  &:hover {
    color: darken(#CC3244, 15%);
  }
}

```


在vue中使用sass

```vue
<style scoped lang="scss">
p:hover {
    color: darken($color: red, $amount: 15%)
}
</style>
```