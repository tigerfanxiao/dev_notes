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

## Pros & State

-   Props 不能被 children 修改, is public
-   State is a snapshot, component manage its own stage, and trigger re-render
-   State optional

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
{props.openSpots === 0 && ( <div className="card--badge">SOLD OUT</div>
)}
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

会触发 render

-   You can only call hooks at the top level of your component or your own hooks.
-   You cannot call hooks inside loops or conditions.
-   You can only call hooks from React functions, and not regular JavaScript functions.

Example

-   An input text field
-   Any text that has been entered into the field
-   A Reset button to set the field back to its default state

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

### useEffect

用于 side effect, 比如 component mount 的时候, 就运行某个动作. 而不是等待用户点击触发时间才运行.

```jsx
useEffect(function () {
    getAdvice(); // 被side effect 是被调用的函数
}, []); // 设计的 dependence
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
import "./App.css";
import { useState } from "react";
import { validateEmail } from "./utils";

const PasswordErrorMessage = () => {
	return (
		<p className="FieldError">Password should have at least 8 characters</p>
	);
};

function App() {
	const [firstName, setFirstName] = useState("");
	const [lastName, setLastName] = useState("");
	const [email, setEmail] = useState("");
	const [password, setPassword] = useState({
		value: "",
		isTouched: false,
	});
	const [role, setRole] = useState("role");

	const getIsFormValid = () => {
		return (
			firstName &&
			lastName &&
			validateEmail(email) &&
			password.value.length > 8 &&
			role !== 'role'
		);

	};

	const clearForm = () => {
		setFirstName("");
		setLastName("");
		setEmail("");
		setPassword({
			value: "",
			isTouched: false,
		});
		setRole("role");

	};

	const handleSubmit = (e) => {
		e.preventDefault(); // 阻止默认行为
		alert("Account created!");
		clearForm();
	};

	return (
		<div className="App">
			<form onSubmit={handleSubmit}> // 处理提交动作
				<fieldset>
					<h2>Sign Up</h2>
					<div className="Field">
						<label>First name <sup>*</sup></label>
						// input 需要输入两个 props: value 和 onChange
						<input
							value={firstName}
							onChange={e=>setFirstName(e.target.value)}
							placeholder="First name"
						/>
					</div>

					<div className="Field">
						<label>Last name</label>
						<input
							value={lastName}
							onChang={(e)=>setLastName(e.target.value)}
							placeholder="Last name" />
					</div>

					<div className="Field">
						<label>Email address <sup>*</sup></label>
						<input
							value = {email}
							onChange={e=>setEmail(e.target.value)}
							placeholder="Email address" />
					</div>

					<div className="Field">
						<label>Password <sup>*</sup></label>
						<input
							value = {password}
							onChange={e=>setPassword({...password, value=e.target.value})}
							onBlur={e=>setPassword({...password, isTouched=true})}
							placeholder="Password" />
					{ password.isTouched && password.value.length<8 ?(<PasswordErrorMessage/>):null}
					</div>

					<div className="Field">
						<label>Role <sup>*</sup></label>
						<select value={role} onChange={e=>setRole(e.target.value)}>
							<option value="role">Role</option>
							<option value="individual">Individual</option>
							<option value="business">Business</option>
						</select>
					</div>

					<button type="submit" disabled={!getIsFormValid()}>
						Create account
					</button>
				</fieldset>
			</form>
		</div>
	);
}

export default App;
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

# ContextAPI

Context is global state, solve props drilling problem

```jsx

import {createContext, useState, useContext} from "react";

const UserContext = createContext(undefined);

export const UserProvider = ({children})=>{
	const [user] = useState({
		name: "John",
		email: "John@example.com",
	});
	return <UserConext.Provider value={user}></UserContext.Provider>
}

export const useUser = ()=> useContext(UserContext)
```

# build
### public
把图片直接放在 public/images 内
```jsx
// 用绝对路径引入图片
"/images/my.png"
```