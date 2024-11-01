install on ubuntu

```shell
sudo apt update
sudo apt install nodejs
```
check
```shell
node --version && npm --version
```
# npm

```shell
npm init # 交换方式在本地创建 js 项目
npm install # 会把当前目录下的 package.json中的包都安装一下

npm install --save-dev jest # 会在 dev-dependency 下面出现依赖, 只在开发环境使用

```

# package.json


```json
{
	"name": "meta_js",
	"version": "1.0.0",
	"description": "coursera front-end certification",
	"main": "addFive.js", // 程序入口
	"scripts": {
		"test": "jest" // npm run test 用来测试
	},
	"author": "",
	"license": "ISC",
	// 定义了依赖
	"dependencies": {
		"jest": "^29.7.0" 
	}
}
```