### Import react from CDN

React 有三大组件, 需要引入

-   react
-   react dom
-   babel

`index.html`
```html
<html>
	<head>
		// react 服务于 jsx
		<script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
		// react dom
		<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
		// babel
		<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
	</head>
	<body>
		<div id="root"></div>
		<script src="index.js" type="text/babel"></script>
	</body>
</html>
```

`index.js`
```jsx
import ReactDOM from "react-dom/client";

ReactDOM.render(<h1>hello world</h1>, document.getElementById("root"));
```

### Import react 17
```jsx
import React from "react" // 服务于 JSX 语法
import ReactDOM from "react-dom"

ReactDOM.render(<h1>hello world</h1>, document.getElementById("root"));
```
### import react 18

```jsx
import React from "react"
import ReactDOM from "react-dom/client" // 新变化
// 直接创建 root 元素
ReactDOM.createRoot(document.getElementById("root")).render(nav)
```


# Code Sandbox

codesandbox.io
https://codesandbox.io/p/sandbox/react-first-app-advice-forked-3s85sm
# Icons
https://react-icons.github.io/react-icons/icons/fa6/

# Initiate Project

-   使用 react-app 构建项目

```shell
npm init react-app <app_name>
cd <app_name>
npm start # 开启服务器
```

-   在当前目录下创建项目

```shell
npm init react-app .
npm start
```

### Create project Vite
- Bundle
- Babel

### nvm
nvm 可以在一台设备上安装多个 node 版本, 有点像 python 的虚拟化环境

install node with nvm
```shell
nvm install --lts
```

```shell
# install node
node -v
npm -v # install npm automatically with node
```

create project
```shell
npm create vite@lastest
# 选择项目类型

cd project_name
npm install # 安装 package.json
npm run dev # 运行
```

### pack project

```shell
npm run build
```
- 会增加一个 index.pack.js 文件
- 不需要 node_modules
# Concepts

component based architecture
### Virtual DOM
JSX 只能返回一个 javascript object, 浏览器是看不懂的, 即使用 `JSON.stringify(obj)`, 也只能看到对象的内容. 浏览器的 DOM 无法直接解析.  只有使用 `ReactDOM.render()`才能使浏览器 DOM 变化
### JSX
- 必须在一个 parent tag 内
```jsx
<>
	<h1></h1>
	<ul></ul>
</>
```

所有的 component JSX 文件首字母大写
function name 首字母大写
transpiler, 使用 babel 把 ES6 和 JS 代码编译成浏览器可以读懂的 JS 代码

App.js 最大的 component
但是根节点是 root

# JSX

可以在一个文件里写 JS, HTML, CSS, 是构造 component 必须要的元素

### Children

```jsx
const h1 = document.createElement("h1");
h1.textContent = "Hello world";
h1.className = "header";
console.log(h1);
// <h1 class="header">

const element = <h1 className="header">This is JSX</h1>;
console.log(element);

/*
1. 可以看到 jsx 元素构建的 html 元素里面必然有 props 属性
2. props 属性下, 里面必然有 children 属性
3. 如果 children 属性只有一个元素, 则不是列表
{
	type: "h1",
	key: null,
	ref: null,
	props: { 
		className: "header", 
		children: "This is JSX" 里面有所有子元素, 包括 contentText
	},
	_owner: null,
	_store: {}
}
*/
```

如果 children 属性有多个元素, 才是 list

```jsx
const page = (
    <div>
        <h1 className="header">This is JSX</h1>
        <p>This is a paragraph</p>
    </div>
);

console.log(page);

ReactDOM.render(page, document.getElementById("root"));

/*
{
	type: "div", 
	key: null, 
	ref: null, 
	props: {
		children: [
			{
				type: "h1", 元素结构保持一致
				key: null, 
				ref: null, 
				props: { 
					每个元素都有 props 属性, 到最下层,会有一个不是 list 的 children 属性
					className: "header", 
					children: "This is JSX"
					}, 
				_owner: null, 
				_store: {}
			}, 
			{
				type: "p", 
				key: null, 
				ref: null, 
				props: {
					children: "This is a paragraph"
					}, 
				_owner: null, 
				_store: {}
			}
		]
	
	}, 
	_owner: null,
	_store: {}
}
*/
```

对于自定义的 component, 也可以使用 children

### Customised Component

#### function component
1. function name should be capitalised
2. return one `<div>`
3. render like `<Header />`

File `app.js`

```jsx
// 构建自己的 component
function Head() {
    return (
	    <h1>Hello world</h1>
	);
}

function App() {
    return <Head />; // render Head Component
}
export default App; // 导出默认 module
```

File `index.js`

```javascript
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

### Organize component

`Header.jsx`
```jsx
import React from "react" // 从我安装的库中找

function Header() {
	return ()
}

export default Header 
```
import Header
```jsx
import Header from "./Header" // 从我本地的文件找
```

### ClassName

使用 className attribute in JSX

```jsx
<p className="link">Read more...</p>
```

## Pros vs State

- Pros 用于父节点到子节点的数据传递. 而 state 主要用于 component 内部的状态保存
- Props 不能被 children 修改, is public
- State is a snapshot, component manage its own stage, and trigger re-render
- State optional

### Props

从 JSX 转变为 pure JS

```jsx
function App() {
    return <h1>Hello there</h1>;
}
```

转变为

```javascript
React.createElement(
    type, // h1
    [props], // null
    [...children] // inner content
);
```

parent component one-direction transfer data to child component

-   只能单向传递, 从 parent 到 child, 不能反过来
-   props 在传递的过程中, 不能被修改, 也称为 pure function
file `index.js`

```jsx
import App from "./App.js";

ReactDom.createRoot(
    document.querySelector("#root") // search by id
).render(<App title="welcome" />); // pass pros title=welcome to component App
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
            <Card 
	            img="katie-zaferes.png"
				rating={5.0}
				reviewCount={6} // 把 6 解释成 JS 的数字,而不是字符串
				country="USA"
				title="Life Lessons with Katie Zaferes"
				price={136}
	        />
            
    );
}
export default App;
```

Card.js

```jsx
import PropTypes from "props-types";

function Card(props) {
    return (
        <div className="card">
            {" "}
            // 定义 css
            <h2>{props.img}</h2> // 从 parent component 获取数据
            <h3>{props.rating}</h3>
            <h3>{props.reviewCount}</h3>
            <h3>{props.country}</h3>
            <h3>{props.title}</h3>
            <h3>{props.price}</h3>
        </div>
    );
}

Card.propTypes = {
	img: PropTypes.string.isRequired,
	rating: PropTypes.number.isRequired,
	reviewCount: PropTypes.number.isRequired,
	country: PropTypes.string.isRequired,
	title: PropTypes.string.isRequired,
	price: PropTypes.number.isRequired,

};
export default Card;
```

### map component
```jsx
import React from "react"
import Joke from "./Joke"
import jokesData from "./jokesData"

export default function App() {
	const jokes = jokesData.map(joke =>{
		return (<Joke
			setup={joke.setup}
			punchline={joke.punchline}/>
		)
	})

	return (
		<div> 
			{jokes} // 这里的 jokes 是一个序列
		</div>)
}
```

### Conditional render

```jsx
// show something or not
{state && <div>something</div>}
// show this or that
{state ? "this thing":"that thing"}
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
import avatar from "./avatar.png";

function Logo(props) {
    const userPic = <img src={avatar} />;
    return userPic;
}
```

### CSS in JSX

-   change hyphen to CamelCase
-   change ; to
-   change to object

```jsx
function Promo(props) {
    const styles = {
        color: "tomato", // 修改成 object
        fontSize: "40px", // CamelCase
    };

    return (
        <div className="promo-section">
            <div>
                <h1 style={styles}>
                    {" "}
                    // 定义 attribute
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
    );
};

// single parameters
const Nav = (props) => (
    <ul>
        <li>{props.name}</li>
    </ul>
);
// any number of parameters
// implicit return
const Nav = () => (
    <ul>
        <li>Home</li>
    </ul>
);
```

more example for arrow function

```javascript
[10, 20, 30].forEach((item) => item * 10);
```

### Tenary Operator

```jsx
name == Bob ? "Yes, it is Bob" : "I don't know this person";
```

# Event

### MouseEvent

-   onClick

### Clipboard Events

-   onCopy
-   onCut
-   onPaste

# Hook

### useState hook
- useState 的初值 `['', f()]`
- 提供一个 callback function 给 `f(oldStateValue)`
- 会触发 render
-   You can only call hooks at the top level of your component or your own hooks.
-   You cannot call hooks inside loops or conditions.
-   You can only call hooks from React functions, and not regular JavaScript functions.

Example

-   An input text field
-   Any text that has been entered into the field
-   A Reset button to set the field back to its default state

如果需要知道之前最新的状态, 需要传入 callback 函数给 setState
```jsx

import React from "react"

export default function App() {
	const [count, setCount] = React.useState(0)
	function add() {
		setCount(prevCount => prevCount + 1)	
	}
	function subtract() {
		setCount(prevCount => prevCount -1)
	}
	return (
		<div className="counter">
			<button className="counter--minus" onClick={subtract}>–</button>
			<div className="counter--count">
				<h1>{count}</h1>
			</div>
			<button className="counter--plus" onClick={add}>+</button>
		</div>
	)
}
```

 如果不需要知道最新的状态, 可以直接传值给 SetState
```jsx
import { useState } from 'react';

export default function InputComponent()
	const [inputText, setText] = useState('hello'); // hello is default value for inputText
	function handleChange(e) {
		setText(e.target.value); // 把 inputText 对象取出来
	}

	return (
		<>
			<input value={inputText} onChange={handleChange} />
			<p>You typed: {inputText}</p>
			// 设置为初始值
			<button onClick={() => setText('hello')}>Reset</button>
		</>
	);

}
```

### useRef hook

-   `ref.current.focus()`
-   `ref.current.value()`
    和 useState 最大的区别在于不会触发 render
    控制 dom

```jsx
function TextInputWithFocusButton() {
    const inputEl = useRef(null);
    const onButtonClick = () => {
        // `current` points to the mounted text input element
        inputEl.current.focus();
    };

    return (
        <>
            {" "}
            // ref attribute works for useRef hook
            <input ref={inputEl} type="text" />
            <button onClick={onButtonClick}>Focus the input</button>
        </>
    );
}
```

### useEffect hook

用于 side effect
- component mount 的时候, 就运行某个动作. 而不是等待用户点击触发时间才运行.
- local Storage
- 外部 API 调用
- 给 windows 添加 event listener


```jsx
React.useEffect(function() {
	// 被side effect 是被调用的函数
	fetch("https://swapi.dev/api/people/1")
		.then(res => res.json())
		.then(data => console.log(data))
}, [count])  // [] 表示只跑一次

// 用 async await 改写
React.useEffect(async () => {
		const res = await fetch("https://api.imgflip.com/get_memes")
		const data = await res.json()
		setAllMemes(data.data.memes)
	}, [])
```

cleanup
```jsx
import React from "react"

export default function WindowTracker() {
	const [windowWidth, setWindowWidth] = React.useState(window.innerWidth)

	React.useEffect(() => {
		// 定义绑定到window listender上的callback 函数
		function watchWidth() {
			console.log("Setting up...")
			setWindowWidth(window.innerWidth)
		}
		// 在第一次加载 component 时,给 windows 添加 event listener
		window.addEventListener("resize", watchWidth)
		// 当这个 component 别关闭时, 运行的 callback 删除 listener
		return function() {
			console.log("Cleaning up...")
			window.removeEventListener("resize", watchWidth)
		}

	}, [])

	return (
		<h1>Window width: {windowWidth}</h1>
	)

}
```

### useContext hook
Context is global state, solve props drilling problem
在顶部使用
```jsx
// 创建一个 context
const ThemeContext = React.createContext();

export default function App() {
	return (
		// 需要用 Provider 把元素 最上层的 component 包起来
		// 用过 value 给 context 添置值
		<ThemeContext.Provider value = "light">
			<div className="container dart-theme">
				<Header />
				<Button />
			</div>
		</ThemeContext.Provider>
	)
}
// 导出 ThemeContext 给别的 module 使用
export { ThemeContext }
```

传一个值, 传一个object 的不同写法
```jsx
// 传一个值
<ThemeContext.Provider value = "light">
// 传一个变量, 因为是 js 所以需要使用 {}
<ThemeContext.Provider value = { theme }>
// 传一个 object, 第一个{} 表示 js, 第二个{} 表示 object
<ThemeContext.Provider value = {{ theme, toggleFunc }}>
// 上面写法等于
<ThemeContext.Provider value = {{ theme: theme, toggleFunc: toggleFunc }}>
```

使用 context

```jsx
// 从别的模块引入context
import { ThemeContext } from "./App";

export default function Button() {
	// 获取 context 中的值
	const value = React.useContext(ThemeContext);
	return (
		<button className={`${value}-theme`}>
			Switch Theme
		</button>
	)

}
```

### dot syntax for compound components
如果 menu 下还有多个子 component. 在 Menu 文件夹下有如下结构

```
Menu
	- Menu.js
	- MenuButton.js
	- MenuDropdown.js
	- MenuItem.js
	- index.js # 创建这个 js 文件来做转化

```

Menu文件夹下的 `index.js`
```js
import Menu from "./Menu"
import MenuButton from "./MenuButton"
import MenuDropdown from "./MenuDropdown"
import MenuItem from "./MenuItem"

Menu.Button = MenuButton
Menu.Dropdown = MenuDropdown
Menu.Item = MenuItem

export default Menu
```

使用 dot syntax
```jsx
// 只要引入 index.js
import Menu from "./Menu/index"

function App() {
	const sports = ["Tennis", "Pickleball", "Racquetball", "Squash"]

	return (
		<Menu>
			<Menu.Button>Sports</Menu.Button>
			<Menu.Dropdown>
				{sports.map(sport=>(
					<Menu.Item key={sport}>{sport}</Menu.Item>
				))}

			<Menu.Dropdown>
		</Menu>
	)
}
```

### route

安装 routeReactDom

```shell
npm install react-router-dom@6
```

```jsx
import "./App.css";
import Homepage from "./Homepage";
import AboutLittleLemon from "./AboutLittleLemon";
import Contact from "./Contact";
import { Routes, Route, Link } from "react-router-dom";

function App() {
    return (
        <div>
            <nav>
                <Link to="/" className="nav-item">
                    Homepage
                </Link>
                <Link to="/about" className="nav-item">
                    About Little Lemon
                </Link>
                <Link to="/contact" className="nav-item">
                    Contact
                </Link>
            </nav>
            <Routes>
                <Route path="/" element={<Homepage />}></Route>
                <Route path="/about" element={<AboutLittleLemon />}></Route>
                <Route path="/contact" element={<Contact />}></Route>
            </Routes>
        </div>
    );
}

export default App;
```

index.js

```jsx
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```

### image

```jsx
import avatar from "./assets/logo.png";

function UserImage() {
    return (
        <div>
            <img src={avatar} alt="User image" />
        </div>
    );
}
export default UserImage;
```

### media package

```shell
npm install react-player
```

```jsx
import ReactPlayer from "react-player";
import ReactPlayer from "react-player/youtube"; // if only youtube video

const MyVideo = () => {
    return <ReactPlayer url="https://www.youtube.com/watch?v=ysz5S6PUM-U" />;
};

const App = () => {
    return (
        <div>
            <MyVideo />
        </div>
    );
};

export default App;
```

### audio

```jsx
import React from "react";

function App() {

	const bird1 = new Audio("https://upload.wikimedia.org/wikipedia/commons/9/9b/Hydroprogne_caspia_-_Caspian_Tern_XC432679.mp3");

function toggle1() {
	if (bird1.paused) {
		bird1.play();
	} else {
		bird1.pause();
	}
	return (
		<div>
			<button onClick={toggle1}>Caspian Tern 1</button>
		</div>

	);
};

export default App;
```

## Form

change a form from uncontrolled component to controlled component

1. 使用 useState 来控制输入
2. input 中需要输入两个值 value, onChange
3.

```jsx
import React from "react"

  
export default function Form() {
	// 使用 state object 来控制所有输入参数
	const [formData, setFormData] = React.useState({firstName: "", lastName: ""})

	console.log(formData)

	function handleChange(event) {
		const {name, value, type, checked} = event.target
		setFormData(prevFormData => {
			return {
				...prevFormData, // 之前的值
				// 用[]可以动态的使用变量作为对象的key 值
				[name]: type === "checkbox"? checked: value
			}
		})

	}

	return (
		<form>
			<input
				type="text"
				placeholder="First Name"
				onChange={handleChange}
				name="firstName" // this property identify the event.target
				value={formData.firstName}
			
			/>
			<input
				type="text"
				placeholder="Last Name"
				onChange={handleChange}
				name="lastName"
				value={formData.lastName} // single source of truth, 不让 input 自己去维护一个 state
			/>
		</form>
	)
}
```
下面的 form 元素和 html 中的写法略有不同
```jsx
<textarea/>

<input 
	type="checkbox" 
	id="idFriendly" 
	checked={formData.isFriendly}
	name="idFriendly"
	onChange={handleChange}
/>
<label htmlFor="idFriendly">Are your friendly</label>


```

radio
```jsx

<fieldset>
	<legend>Current employment status</legend>
	<input
		type="radio"
		id="unemployed"
		name="employment"
		value="unemployed"
		checked={formData.employment === "unemployed"}
		onChange={handleChange}
	/>
	<label htmlFor="unemployed">Unemployed</label>
	<br />
	
	<input
		type="radio"
		id="part-time"
		name="employment"
		value="part-time"
		checked={formData.employment === "part-time"}
		onChange={handleChange}
	/>
	<label htmlFor="part-time">Part-time</label>
	<br />

	<input
		type="radio"
		id="full-time"
		name="employment"
		value="full-time"
		checked={formData.employment === "full-time"}
		onChange={handleChange}
	/>
	<label htmlFor="full-time">Full-time</label>
	<br />
</fieldset>
```

dropdown
```jsx
<label htmlFor="favColor">What is your favorite color?</label>
<br />
<select
	id="favColor"
	value={formData.favColor}
	onChange={handleChange}
	name="favColor"
>
	<option value="">-- Choose --</option>
	<option value="red">Red</option>
	<option value="orange">Orange</option>
	<option value="yellow">Yellow</option>
	<option value="green">Green</option>
	<option value="blue">Blue</option>
	<option value="indigo">Indigo</option>
	<option value="violet">Violet</option>

</select>
```

邮箱验证

```jsx
export const validateEmail = (email) => {
    return String(email)
        .toLowerCase()
        .match(
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        );
};
```

submit
```jsx
function handleSubmit(event) {
	// 阻止默认行为, 因为默认情况下, submit 会刷新页面, 充值所有 input 内容. 把所有参数都放在 url 里,用 post 方式发送出去
	event.preventDefault()
	console.log(formData)
}

return (
	// 在 form 标签下配置 onSumit 方法
	<form onSubmit={handleSubmit}>
		// 在 form 中的 button, 默认是 type=submit, 会触发form 递交 event
		<button>Submit</button>

```

# build
### public
把图片直接放在 public/images 内
```jsx
// 用绝对路径引入图片
"/images/my.png"
```

# 3rd Party Module
### classnames & clsx
```shell
npm install classnames
```
把多个 classname 进行拼接
```jsx
export default function Button({children, className, size, ...rest}) {

	const allClasses = classnames(sizeClass, className)
	//  button-large green
}
```

