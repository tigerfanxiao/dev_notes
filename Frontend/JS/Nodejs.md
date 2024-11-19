# NPM
### Install Nodejs
install on ubuntu

```shell
sudo apt update
sudo apt install nodejs
```
check
```shell
node --version && npm --version
```

### Initiate project
1. 构建环境
```shell
# 创建项目
npm init -y
# 会把当前目录下的 package.json中的包都安装一下
npm install --save-dev vite # 使用 vite 服务器
npm install --save-dev jest # 使用 jest 测试包

# 配置 package.json 文件
# 启动项目
npm start # 启动 vite 服务器
```
 2. package.json
```json
{
	"name": "meta_js", // 项目名称
	"version": "1.0.0",
	"description": "description of project",
	"main": " index.js", // 程序入口的 js 文件
	"scripts": {
		"test": "jest", // npm run test 用来测试
		"start": "vite" // npm start 启动 vite 服务器
	},
	"author": "",
	"license": "ISC",
	// 定义了依赖
	"dependencies": {
		"jest": "^29.7.0" 
	}
}
```
3. index.html
```html
<!-- 确保程序有正确的入口 -->
<script src="index.js" type="module"></script>
```

