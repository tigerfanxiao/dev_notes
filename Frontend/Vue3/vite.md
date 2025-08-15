
对js文件进行打包


  
安装
```shell

npm create vite@latest
# 设置项目名
# 选择框架
cd project_name

npm install  # 安装开发环境
```


默认情况下， css文件是通过 main.js 引入的。 Vanilla JS是不允许这种操作的。 这是vite做的


vite script

```shell
npm run dev # lauch server for dev, 任何改动会自动刷新
npm run build # exit dev state, export to dist folder
npm run preview # launch server from distribution directory

```