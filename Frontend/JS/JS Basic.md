### import script to html
```html
<script src="script.js"></script>
```
# VSCode Configuration

插件
code runner 可以在 vscode 里直接运行 js 代码 `control + option + n`


### `console.log()`
```javascript
// print with style in browser
console.log("%cHello, World", "color: blue; font-size: 40px");

// print one line, seperate by space
console.log("Hello ", "there, ", "World")
```

### comments

```javascript
// one line commments

/*
multiple line comments
*/
```
### String
```javascript
// Concatinate String
"xiao" + "fan"
"xiao".concat("fan")
"Xiao".charAt(0) // 第 0 个元素
"Xiao".slice(1) // 第一个元素后面所有的元素, 包含第一个
'xiao'.toUpperCase();
'XIAO'.toLowerCase();
'xiao'.indexOf('x') // 0
'h0-ho'.split("-")
// length of string
"ABCD".length
// index of string
"ABC"[1]

// search for string
"hello".match("hel") //[ 'hel', index: 0, input: 'hello', groups: undefined ]
```
### template literals
```javascript
let name = 'Xiao';
console.log(`Hello, World!${name}`);

let multiple_line = `
   fan
   xiao
`;
console.log(multiple_line);
```
### Date
```javascript
const date = new Date()
date.getHours() // 时
```
## Array
```javascript
// check if element in the array
arr.include(ele)

// add ele to the end
arr.push();
// add ele to the first
arr.unshift();

// remove the last element
fruits.pop();
// pop the first element
arr.shift();

```
Array iteration
### forEach
```javascript
// forEach 
const fruits = ['kiwi','mango','apple','pear'];
function appendIndex(fruit, index) {
	console.log(`${index}. ${fruit}`)
}
fruits.forEach(appendIndex);

// forEach with index
const veggies = ['onion', 'garlic', 'potato'];
veggies.forEach( function(veggie, index) {
	console.log(`${index}. ${veggie}`);
});
```
### `.find()`
find the first element that meets the condition
```js
const nums = [0,10,20,30,40,50];
select_num = nums.find(num=> num === 10);
```
### `.filter()`
```javascript
const nums = [0,10,20,30,40,50];

nums.filter(num=>num>20)

```
### `.map()`
```javascript
[0,10,20,30,40,50].map( 
	function(num) {
		return num / 10;
	}
)
```
### Map
与 Object 的区别在于没有继承性, 没有prototype, 像是字典
```javascript
let bestBoxers = new Map();

bestBoxers.set(1, "The Champion");
bestBoxers.set(2, "The Runner-up");
bestBoxers.set(3, "The third place");

console.log(bestBoxers);
bestBoxers.get(1); // 'The Champion'
```
### Set
```javascript
const repetitiveFruits = ['apple','pear','apple','pear','plum', 'apple'];
const uniqueFruits = new Set(repetitiveFruits);
console.log(uniqueFruits);

// add number to set
let mySet = new Set();
mySet.add(1);
mySet.add(2);

mySet.has(3) // check if element in the set, but hashtable

```
### spread operator

```javascript
const myArray = [1, 3];
function add(a, b) {
	return a + b;
}

let result = add(...myArray);
console.log(result);

```
concatenate two array
```javascript
const fruits = ['apple', 'pear', 'plum']
const berries = ['blueberry', 'strawberry']
const fruitsAndBerries = [...fruits, ...berries]
```
convert string to array
```javascript
const greeting = "Hello";
const arrayOfChars = [...greeting];

console.log(arrayOfChars); //  ['H', 'e', 'l', 'l', 'o']
```
concatenate two object
```javascript
const flying = { wings: 2 }
const car = { wheels: 4 }
const flyingCar = {...flying, ...car}

console.log(flyingCar) // {wings: 2, wheels: 4}
```
copy object
```javascript
const car1 = {
	speed: 200,
	color: 'yellow'
}

const car 2 = {...car1}
```

### rest operator
```javascript
const meal = ["soup", "steak", "ice cream"]
let [starter] = meal;

console.log(starter); // soup
```

### Variable

```javascript
// 声明变量
var v_1; 
var v_1 = 'xiao'; // global scope
const v_1 = 'fan'; // block scope, assign during declaration. cannot change
let v_2; // block scope


style = `color: ${color}`;
```

### Scope
js 会先在 block scope 的本地找, 然后再去 global scope 去找

```javascript
var globalVar = 77;
function scopeTest() {
	var localVar = 88; // localVar is only defined inside of block scope
}

console.log(localVar); // localVar is not defined in global scope
```


```javascript
function scopeTest() {

var y = 44;
	console.log(x);
}

var x = 33;
scopeTest(); // 33 , x is defined in global scope
```
### Object
```javascript
var user = {
	property_1: 1,
	property_2: "xiao",
};

// add more properties to the object
var house2 = {};
house2["rooms"] = 4;
house2['color']= "pink";

// call the property
houre2.rooms // 4

// define method
car.lightsOn = function() {
	console.log("The lights are on.")
}

```

type of object
```js
typeof('hello') == 'string' // String
```

es6 create object
```js
// if object property has the same name of variable
a = "a letter"
b = "b letter"
{a, b}

```
### Object.assign
```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

// create new object and add new properies
const result = Object.assign(target, source);
console.log(result); // { a: 1, b: 4, c: 5 }
```

### Math

```javascript
// between 0 and 0.99
Math.random();
Math.ceil(2.1) // 3

```
### Empty Values
null 没找到
```javascript
var letter = 'abc';
letters.match(/d/); // 没找到, 返回 null
```
undefined 没有定义. 所有函数返回 undefined
```javascript
var a;
a // undefined

```

### Datatype
```javascript
typeof(11) == number
typeof('hello') == string
typeof(null) == null
typeof(undefined) = undefined
// ES6 之后出现的 Data type
BigInt 

Symbol
```

### Operation
```javascript

a > 5 && a < 10
a > 5 || a > 10
!(a > 5)

10 == "10" // convert type, true
10 === "10" // not convert type, false
!== // not equal

```

### Ternary
```js
theme === 'light'? 'dark':'light'
```
### Condition
```javascript
if (condition == true) {
	// execute code
} else if (condition == true) {
   // code
} else {

}
```

### switch
```javascript

switch(h1.innerText) {
	case arr[0]:
		h1.innerText = arr[1]
		break
	case arr[1]:
		h1.innerText = arr[2]
		break
	default:
		h1.innerText = arr[0]
}

```

### Loop

```javascript
var i = 1;
while (i< 4) {
	console.log(i);
	i += 1;
}

for (var i = 1; i <= 3; i++) {
	console.log(i);
}

```
### for - of loop
for-of loop used for iterable data type
```javascript
// Prototype only has engine property
const car = {
	engine: true
}

const sportsCar = Object.create(car); 
sportsCar.speed = "fast"; // add speed property to sportsCar object
console.log(sportsCar); // {speed: "fast"}

// for-in loop will also go through prototype
for (prop in sportsCar) {
	console.log(prop); // speed, engine 
}
// for-of loop will only loop current obejct
for (prop of Object.keys(sportsCar)) {
	console.log(prop, sportsCar[prop]); // speed: fast
}

for (let item of b) {
  // do somethin
}
```
### Function

```javascript

function myFunc() {
 console.log('hello world');
}

myFunc();
```

### Error Handling
```javascript
try {
	// do 
} catch (err) {
	// do 
}

// throw a error obejct
throw new ReferenceError();

```
### Class
```javascript
class Car {
	constructor(color, speed) {
		this.color = color;
		this.speed = speed;
	}
	// method
	turboOn() {
		console.log("turbo is on!")
	}
}

new Car('red', '100')
```

class inheritance
```javascript
class Animal {
	constructor(color = 'yellow', energy = 100) {
		this.color = color;
		this.energy = energy;
	}

	isActive() {
		if(this.energy > 0) {
			this.energy -= 20;
			console.log('Energy is decreasing, currently at:', this.energy)
		} else if(this.energy == 0){
			this.sleep();
		}
	}

	sleep() {
		this.energy += 20;
		console.log('Energy is increasing, currently at:', this.energy)
	}

	getColor() {
		console.log(this.color)
	}

}

class Cat extends Animal {
	constructor(sound = 'purr', canJumpHigh = true, canClimbTrees = true, color, energy) {
	super(color, energy);
	this.sound = sound;
	this.canClimbTrees = canClimbTrees;
	this.canJumpHigh = canJumpHigh;
}

	makeSound() {
		console.log(this.sound);
	}

}
```

### Destructure

```js
const person = {
	img: "./images/im.jpg",
	name: "xiao",
	phone: "88888",

}
{img, name} = person // 可以只取出两个值, 但是 key 值必须相同
```

重新构建对象
```js
let password = {
	value: "",
	isTouched: false;
}
// value会被覆盖
let newPassword = {...password, value:e.target.value}

```

挑出某一个元素
```javascript
let { PI } = Math // 从 Math module 里面挑出 PI
```

交换两个元素
```js
[ nums[i], nums[j]] = [ nums[j], nums[i]];
```
React Props
```jsx
card = {
	id = 5,
	name = 'black',
}
<Card {...card}/>
```
### Object 
object.create()
```javascript
const car = {
	enginee: true, 
	steering: true, 
	speed: "slow"
}

const sportsCar = Object.create(car)
```

Object.keys(), Object.values(), and Object.entries().
```javascript
var clothingItem = 
	price: 50,
	color: 'beige',
	material: 'cotton',
	season: 'autumn'

}

for(const key of Object.keys(clothingItem) ) {
	console.log(key, ":", clothingItem[key])
}
```
## arrow function
```js
const add = (a, b) => a + b;
```
### Proxy
Proxy有点像是python中的dunner方法，或者getter， setter方法，用来增加或者扩展对象的基本行为。

```javascript

const gameSettings = { date: "2019-12-30"};
const gameSetitngsProxy = new Proxy(gameSettings, {
	// 取对象中的元素，如果key存在，则返回value， 如果不存在，则返回fanxiao
	get: (o, property) => {
		return property in o ? o[property] : "No such key"
	}, 

	set: (o, property, value) => {
		if (property === 'difficulty' && !['easy', 'medium', 'hard'].includes(value.toLowerCase())) {
			throw new Error('Difficulty is invalid');
		}
		o[property] = value
	}
});

console.log(gameSettingsProxy.date); // 2019-12-30
console.log(gameSettingsProxy.difficulty); // No such key 


gameSettingsProxy.difficulty = "easy" // 成功

```

Vue使用了proxy做数据的双向绑定


# BOM

```javascript
setTimeout(callback, 3000)  // 3秒后运行
```

window
```js

window.addEventListener("resize", function() {
	console.log("Resized")
})
```
### LocalStorage
```js
localStorage.getItem("key")
// 如果找不到, return null
localStorage.setItem("key", value)

// 注意 value 必须是 string, 如果是其他复杂对象, 必须使用
JSON.stringify(value)
// 从 localstorage 取出对象
JSON.parse(stringifiedValue)
```

# DOM

### select element
```javascript
document // DOM 
document.querySelector('p') // get the first found p tag
document.querySelectorAll('p') // get all p tags
document.getElementById('id')
document.getElementByClassName('myClass')


```
### append element
```js

// create element
const h1 = document.createElement('h1');
h1.textContent = 'hellow'
h1.className = 'h1header'
h1.setAttribute('type', 'text') // 或者

document.getElementById('root').append(h1) // 在指定元素内添加
document.body.appendChild(h1); // 在 body 中添加
```
### set element attribute
```javascript
var input = document.createElement('input')
input.setAttribute('type', 'text')
var a = input.value

})
```
# Module

导出 `myFunc.js`
```javascript
function myFunc() {
 //
}

module.exports = myFunc // this function can be import by other module
```
导入

```javascript
const myFunc = require('./myFunc')
```

#### default export
**one default export** per JavaScript module.
```javascript
export default function addTwo(a, b) {
	console.log(a + b);
}

// 先定义, 后导出
function addTwo(a, b) {
	console.log(a + b);
}
export default addTwo;
```

#### export multiple
```javascript
export function addTwo(a, b) {
	console.log(a + b);
}

export function addThree(a, b, c) {
	console.log(a + b + c);
}

// 
function addTwo(a, b) {
	console.log(a + b);
}

function addThree(a, b, c) {
	console.log(a + b + c);
}
export { addTwo, addThree };
```

import module

```javascript
// import default
import addTwo from "./addTwo";
// import one from multiple export
import { addTwo } from "./addTwo";
```
# Events

只能在正式 html 元素才能用 onClick 属性. 在自定义的 componemt 中, 自己定义的 handleClick 只是一个普通的 props, 需要借助传入的callback 函数来定义
event type: click
```javascript
const target = document.querySelector('body');
function handleClick() {
	console.log('hello world');
}
target.addEventListener('click', handleClick)
```

# JSON
```javascript
const json_str = '{"greeting": "hello"}'
const json_obj = JSON.parse(json_str)

// serialization, exclude the function from the object automatically
JSON.stringify(js_obj)
```


# ES6
### compute property syntax
```js
// 之类 name 可以是一个变量
{
	[name]: value 
}
```
### implicit return 

```js
// 即使在两行, 用括号就不用写 return
user => ({
name: "xiao"
})
```