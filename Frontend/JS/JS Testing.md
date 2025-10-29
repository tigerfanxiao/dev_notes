
JEST 是 JS 的 test framework
JEST 的特性是 code coverage

常见的 test 模式
- unittest
- integration test
- end2end test 



# JEST

## 安装和配置
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

测试文件 `addFive.test.js`
```javascript
const { default: TestRunner } = require("jest-runner");
const addFive = require("./addFive"); // 引入需要测试的函数

test("returns the number plus 5", () => {
	expect(addFive(5)).toBe(10);
});
```

运行测试

```shell
npm run test
```


Mocking
ansychrorous code test


