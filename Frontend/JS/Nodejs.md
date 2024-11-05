install on ubuntu

```shell
sudo apt update
sudo apt install nodejs
```
check
```shell
node --version && npm --version
```
# NPM

### init project

```shell
npm init -y # 交换方式在本地创建 js 项目
# 会把当前目录下的 package.json中的包都安装一下
npm install --save-dev vite # 使用 vite 服务器
npm install --save-dev jest # 使用 jest 测试包

# 配置 package.json 文件
npm start # 启动 vite 服务器
```

package.json

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

