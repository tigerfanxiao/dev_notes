

SASS是pre-process， SASS编译的结果是CSS，所以只能要SASS语法来写
PostCSS是 Post Processor， 优势在于可以用js直接去操作CSS， 所以本质上是一个JS库

实际上工作流是vite compile sass 到css， 然后postcss再处理css， postcss不能直接处理sass

vite应该是已经包含了这个库，为了激活和使用它
在项目根目录下创建 `postcss.config.cjs`

Postcss有很多插件， 这些插件可以用来处理css文件， 下面是插件的查询地址
https://www.postcss.parts/ 

如果我们安装要给postcss插件叫做 autoprefixer
它会给css文件中的一些新的特性（还没有被所有浏览器都支持的，使用的时候需要vendor-prefix)自动补全这些Vendor-prefix

```shell
# 安装这个插件
npm install autoprefixer --save-dev
```

在postcss中引入插件
```javascript
module.exports = {
	
	plugins: [require('autoprefixer')]
}
``` 

重新运行vite 开发环境, 插件就能运行了
可惜目前`::placeholder` 选择器已经被所有浏览器支持了， 不需要vendor-prefix就能使用