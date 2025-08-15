webpack只支持js， 不支持css， scss
需要配置文件webpack.config.js


```javascript

const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const path = require('path');

module.exports = {
  entry: './index.js', // entry post
  output: {
    path: __dirname + '/dist', // where to save the bundle, __dirname是node定义的
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/, // 去除这个文件夹中的所有文件
        use: 'babel-loader' // 用babel包来处理js文件
      },
      {
        test: /\.scss$/, // 这里手动配置support scss 文件
        use: [ // 这些都在package.json中安装了
          MiniCssExtractPlugin.loader, // 将css内容从js文件中取出来
          'css-loader',
          'postcss-loader',
          'sass-loader'
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'main.css'
    })
  ],
  watch: true  // 默认情况下，webpack只会打包一次， 设置watch为true之后， 会一直观察变化
}
```

启动

```shell
npm run start
# 但是这只是打包， 还没有服务器去运行文件
```