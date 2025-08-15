在 JS 中有些固有函数比如 setTimeout, addEventListener 都是异步函数. 也就是说当执行到这个函数式, 引擎会把他们放到Event loop 里面去, 然后继续执行下面的语句. 等到这些程序在 event-loop 中执行完成后再打印出结果

因为这些异步函数执行完成的结果时间不确定, 如果我们有别的函数的执行依赖于这些异步函数的执行的结果. 那么就需要把别的函数的执行语句放入异步函数的 callback 函数中一起执行. 

Callback hell, 如果有多异步函数一级一级的依赖, 就完全无法控制这些函数是否能顺利的执行完成. 因为很多 callback 函数可能存在资源调用的问题. 如果其中一个资源调用失败了. 则整条 callback chain 就失败了

Fix Callback hell with Promise
### Promise
Promis 只有 3 中状态
- pending
- fulfilled
- rejected
```js

const promise = new Promise((resolve, reject) => {
	setTimeout(()=> resolve("done"), 1000);
});

const promise = new Promise((resolve, reject) => {
	setTimeout(() => reject(Error('Promise failed.')), 1000);
});

// 这里定理了callback 函数
promise
	.then(value => console.log(value))
	.catch(error => console.error(error))
	.finally(()=> console.log('done'));
```

### fetch
fetch 返回 promise

```js
fetch('https://jsonplaceholder.typicode.com/posts/1')
	.then(response=>{
		if (!response.ok) throw new Error(response.status)
	})
	.then(reponse=>response.json())
	.then(data => console.log(data.title))
	.catch(error => console.log(error))

```


```js

const blogPost = {
	title: "Cool post",
	body: "lkajsdflkjasjlfda",
	userId: 1,
}

fetch('https://jsonplaceholder.typicode.com/posts', {
	method: "POST",
	headers: {
		"Content-Type": "application/json"
	},
	body: JSON.stringify(blogPost)
	})
	.then(response => response.json())
	.then(data => console.log(data.title))
```



API 资源
https://github.com/public-apis/public-apis
https://api.adviceslip.com/advice
https://jsonplaceholder.typicode.com/posts/1
### Async Await
任何 function 用 async 开头都要返回 promise
```js
// 返回值是给到 resolve
async function getBlogPost() { return 'works here too' } 
// 调用 async 函数, 把结果promise放入 getBlogPost
const getBlogPost = async () => {}
// 用 then 出来 promise
getBlogPost.then(value => console.log(value));
``` 

await 放在 promise 的前面

```js
// 这里只是函数定义, 不是调用函数
async function getPost() {
	try {
		const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
	const data = await response.json();
	console.log(data); // run immediately after await response.json
	} catch (error) {
		console.error("Failed to fetch post")
	}
}
```