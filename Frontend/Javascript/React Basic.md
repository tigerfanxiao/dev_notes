
```shell
npm init react-app <app_name>
cd <app_name>
npm start # 开启服务器

```

# concept
component based architecture
virtual DOM
JSX
所有的 component JSX 文件首字母大写
function name 首字母大写
transpiler, 使用 babel 把 ES6和 JS 代码编译成浏览器可以读懂的 JS 代码

创建项目
```shell
npm init react-app . # 在当前目录下创建项目
npm start
```

App.js 最大的 component
但是根节点是 root
```jsx

function Head() {
	return <h1>Hello world</h1>;
}

function App() {
	return <Head />;  // render Head Component
}
export default App; // 导出默认 module
```

index.js
```javascript
import ReactDOM from 'react-dom/client';
import App from './App';


const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

# JSX
可以在一个文件里写 JS, HTML, CSS, 来构造 component 必须要的元素
### ClassName
使用 className attribute in JSX

```jsx
<p className="link">Read more...</p>
```

### Props
从 JSX 转变为 pure JS
```jsx
function App() {
  return <h1>Hello there</h1>
}
```
转变为
```javascript
React.createElement(
	type, // h1
	[props], // null
	[...children] // inner content
)
```
parent component  one-direction transfer data to child component
- 只能单向传递, 从 parent 到 child, 不能反过来
- props 在传递的过程中, 不能被修改, 也称为 pure function
index.js
```jsx
import App from './App.js'

ReactDom.createRoot(
	document.querySelector('#root') // search by id
).render(<App title="welcome" />) // pass pros title=welcome to component App
```

app.js receive pros 
```jsx
export function(props) { // receive props title = welcome
	return (
		<h1>{props.title}</h1> // props is an object
	);
};
```

example
App.js
```jsx
import "./App.css";
import Card from "./Card";

function App() {
	return (
		<div>
			<h1>Task: Add three Card elements</h1>
			// 引入 3 个 Card component. props 值各不相同
			<Card h2="First card's h2" h3="First card's h3"/>
			<Card h2="Second cards' h2" h3="Second cards's h3" />
			<Card h2="Third card's h2" h3="Third cards'h3" />
		</div>
	);
}

export default App;
```
Card.js
```jsx
function Card(props) {
	return (
		<div className="card"> // 定义 css
			<h2>{ props.h2 }</h2> // 从 parent component 获取数据
			<h3>{ props.h3 }</h3>
		</div>
	);
}

export default Card
```


### html in JSX
```jsx
function Header(props) {
	return ( // 所有的 JSX code 必须放在一个大圆括号里
		<>  // 所有的 html code 都要放在一个<div>里或者 <>里
			<h1 className="head">hello</h1>
			<h2>worlld</h2>
		</>
	) 
}
export default Header
```

### JSX in HTML attribute
```jsx
import avatar from './avatar.png'

function Logo(props) {
	const userPic = <img src={avatar}/>	 ;
	return userPic;
}

```
### CSS in JSX
- change hyphen to CamelCase
- change ; to 
- change to object

```jsx
function Promo(props) {
	const styles = {
		color: "tomato", // 修改成 object
		fontSize: "40px" // CamelCase
	}
	
	return (
		<div className="promo-section">
			<div>
				<h1 style={styles}> // 定义 attribute
					{props.heading}
				</h1>
			</div>
			<div>
				<h2>{props.promoSubHeading}</h2>
			</div>
		</div>
	);
}

```

### Component as Arrow Function

```jsx

const Nav = (props) => {
	return (
	<ul>
		<li>{props.first}</li>
	</ul>
	)
}

// single parameters
const Nav = props => <ul><li>{ props.name }</li></ul>
// any number of parameters
// implicit return 
const Nav = () => <ul><li>Home</li></ul>
```

more example for arrow function
```javascript
[10, 20, 30].forEach(item => item * 10)
```

### Tenary Operator
```jsx
name == Bob ? "Yes, it is Bob" : "I don't know this person";
```


# Event
### MouseEvent
- onClick

### Clipboard Events
- onCopy
- onCut
- onPaste

# Hook
