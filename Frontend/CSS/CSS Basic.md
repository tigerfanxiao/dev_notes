# UI Material

### Icon
https://boxicons.com/?query=dev



### Import CSS
inline 
```css
<div style="front-size: 20px;color: blue">
</div>
```

style tag
```html
<head>
  <style type="text/css">
    /* type属性默认是 text/css 可以省略*/
    h1 {
     color: green;
    }
  </style>
<head>
```

link tag for css file
```css
<head>
	/* type属性默认是 text/css 可以省略*/
	<link rel="stylesheet" type="text/css" href="stylesheet.css">

</head>
```
引入在线的
```css
/*引入在线字体, 400, 700两种粗细样式 */
 <link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Space+Mono:400,700">

```

## Best Practise

### 默认头部配置
本质上，root标签是html， 然后里面有个body标签，但是body标签是有默认margin的。所以下面这个全局配置，会先把所有css对象的margin和情况
```css

* {
    margin: 0;
    padding: 0;
     /* 如果容器的长宽给定, 则配置padding不会使 box变大， 而是压缩内容*/
    box-sizing: border-box; 
}
```

### image on tab
```html
<head>
	<link rel="icon" type="image/svg+xml" href="/vite.svg" />
</head>
```
### class name tradition
```css

如果在 NAVBAR Section里面
.nav--logo_icon

```

### Unit Measurement
- 绝对长度 px
- 相对长途 % em, em units are relative to the **font size of their parent element**.
	- 2em 表示2倍
- rem is relative to the root element
	- If the root font size is 16px (which is common), then 2rem would be 32px (2 × 16px).
- vh viewport
	- 1vh 是 1%的 viewport 
	- 100vh 是 100% viewport
### Box Model

![[Pasted image 20241111075159.png]]
- 默认情况下, padding是零, border是零
- 我们常说的 box, 是只包含 border, 不包含 margin 的. 调整margin不会改变盒子的大小, 但是调整padding时会把盒子变大
- 上下塌陷. 平行两个box之间的margin是叠加的. 上下两个box之间的margin是取上下两个margin中大的一个

```css

/* 定义内容 */
height: 140px; /*内容的高, padding 和 margin 的厚度不计算 */
width:  140px; /*内容的宽*/

/* 定义 padding */
padding-top: 10px 20px 10px;  /* 上面 左右 下面 */

/* 定义 border */
border: solid 3px green; /*虚线dashed 点线 dotted*/
border-top-color: black; /*顶部边框的颜色*/
border-top-width: 10px; /* 底部 border-bottom*/

/* 定义 margin */
/*本元素和其他元素的间隔, 或者浏览器的距离*/
margin: 10px 20px 20px 10px; /* top right bottom left */
margin: 10px 20px 10px; /* top right-left bottom */
margin: 10px auto 10px; /*左右对齐*/
margin: 10px 20px /* top-bottom left-right */
margin: 10px /* all 4 direction*/

```

### box-shadow
```css
box-shadow: 0px 2.98256px 7.4564px rgba(0, 0, 0, 0.1);
```
### HSL
容易修改颜色亮度和饱和度
1st: degree of the hue, and can be between 0 and 360. 
2nd: second and third numbers are percentages representing saturation
3rd: lightness respectively.

### background
```css

height:600px;

/*可以将属性写在一行*/

background-color:antiquewhite;

background-image:url("12.jpg");

background-repeat:repeat;

background-repeat:norepeat; /*不重复*/

background-size: 50px 50px; /*拉伸*/

background-position: left center; 内容的上下和左右位置

/*可以将属性写在一行*/

background: red url("12.jpg") norepeat;
/* 渐变 */
background: linear-gradient(90deg, #672280 1.18%, #A626D3 100%);

```

### text

```css

line-height:100px; /*一行的高度, 文本上下居中*/

vertical-align: -4x; /*从底部向上垂直调整*/

word-spacing:10em; /* 单词间距, 对基本标签同样适用*/

word-spacing:-10em; /* 没有间距, 或者重叠 */

letter-spacing: 10em; /*字母间距 */
letter-spacing: -0.05em; /* 缩小字母间距 */

text-transform: capitalize


```

### font
```css
font-family: Inter, sans-serif;
font-size: 23px;
font-weight: 600;

```
### background
```css
background-image: url('relative path'); /* 平铺填充 */
background-repeat: no-repeat;
background-position: right 75%; /* 第一个参数 x 轴, 第二个参数是 y 轴 */
```
### display

```css

display:none; /*不显示, 元素不占位置*/

display:block; /*取消隐藏, 将标签设置为块标签, 可以调整高度和宽度, 但是独占一行*/

display:inline; /*将标签设为内联标签, 不占一行*/

display:inline-block; /* 是 inline的, 但是可以定义一个元素的height 和 width */
```

inline-block 定义尺寸
```css
span {
  display: inline-block;
  height: 30px;
  width: 20px;
}
```

### overflow
```css

overflow: hidden; /*隐藏超出边界的内容*/

overflow: auto; /*有滚动条*/
overflow-x: auto; /* 如果容器的中的元素横向很多,在超过容器边界时, 会 overflow */
overflow-Y: 'scroll'; 滚动
```

  

#### CSS Floating
使元素平行的方法

方法一

display: inline-block


方法二

使用float属性, 将标签放置在窗口的最左边和最右边

`float`是非完全脱离文档流

`float: left` 如果上面是一个正常流元素, 垂直距离不变, 元素向左漂, 如果上面的元素也是一个float, 则元素会排在上面元素的右面. 元素下面的元素如果是正常流, 会填补原来float元素的位置

`float:right` 浮动要页面右侧, 其他逻辑同上

注意: float元素内的content不会覆盖原文档流中的content文字, 原文档流content会跟在float元素box的后面, 这说明float非完全脱离文档流

**清除float**

`clear:left;`表示该标签左面不能有浮动标签, 如果有则我排在这个浮动标签下面, 这样就清除了浮动

`clear:right;`

`clear:both;` 左右两侧都不能有浮动标签, 否则正常文档流标签会向下移动, 不会和浮动标签重合

例子: 使用clear的方法, 就是添加一个clear空标签

  
```html

<style >

  .clear{clear:both;}

</style>

<div class="clear"></div>

```

  

或者在float标签的父级标签中添加属性

`overflow: hidden;`

  


z-index:10 当设置了fixed之后, 在滚动屏幕时如果遇到了其他设置了position属性值的标签, 会被其遮挡, 这时需要设置z-index.

z-index类似于权重, 设置了z-index属性的元素会优先显示

  


  

#### CSS Global Values

  

```css

 font-family: inherit;

```

  
  
  

# CSS Selector

### Son Select
儿子选择器和层级选择器的区别在于, 如果儿子一层有, 子代选择器就可以找到, 如果儿子一层没有, 但是孙子一层有, 子代选择器就找不到孙子一层
```css
.menu > li {

}
```

### event selector
```css
/*这里a可以是类选择器, 也可以是标签*/

a:hover{} /*在鼠标悬浮的时候*/

a:link{} /*在没有访问的时候*/

a:active{} /*在点击连接的瞬间*/

a:visited{} /*访问过之后*/
```

```css
.container > li {
	/* select all li element under container */
}
```


```css
/* container 下面 div 元素中的第一个, 计数从 1 开始 */
.container > div:nth-child(1) {
	background-color: #96ceb4;  
}
```

  

`!important` 

`#id`

`.class`

host在本地的字符集

[Free Fonts! Legit Free & Quality » Font Squirrel](https://www.fontsquirrel.com/)

  
```css
@font-face {
  font-family: "Roboto";
  src: url("..font/Robot.woff") format('woff2'),
​       url("..font/Robot.woff") format('woff'),
​       url("..font/Robot.woff") format('truetype');
}

```

# Layout
标签级别
div标签本身就是行级标签，会占一行

### relative absolute
absolute 经常和 relative 连用. 配置了 absolute 标签首先会推出当前页面的 flow, 它后面的元素会挤上来. 这个元素会向上级元素查找是否定义了 relative. 如果找到了 relative, 则和这个上级元素进行对其. 如果找不到配置了 relative 的上级元素, 则这个元素就和 body 进行对其. 
- 只有配置了 relative 的元素, 才可以配置 z-index确定页面的层次. 默认是 z-index=0, 隐藏式-1
```css
.container {
	position: relative;
}

.ele {
	position: absolute;
}
```
## Flexbox
- 首先flex 需要配置在 container 上, 这个 container 下面一级的元素设置 `display: flex`, 会把这些元素放在一行里. 注意这个只作用于一级
- 默认情况下, main axis 是横向的, 从左往右. cross axis 是纵向的, 从上往下. 通过 `flex-direction: row` 默认来控制
- 如果 container 的尺寸已经定义了. 内部的元素很大. 在不定义 display 的情况下是会撑大容器的. 但是定了 flex 之后, 就会根据容器的尺寸来
### Flexbox Default Property
```css
flex-direction: row

/* 在flex-direction 是column时 */
align-item: stretch

flex-wrap: nowrap;
```
### flex-direction

```css
flex-direction: column; /* div会占用一行 */
align-item: stretch; /* 默认会做拉长 */

```
![[Pasted image 20241109142419.png]]
### justify-content 
```css
justify-content: flex-start 把子节点往左边放
justify-content: flex-end 把子节点往右边放
justify-content: center 都挤在中间
justify-content: space-around 两个子节点之间空余 2 个单位, 子节点和容器之间空余 1 个单位
justify-content: space-between 两个子节点之间空余 1 个单位, 子节点和容器之间没有空余
justify-content: space-evently 两个子节点之间空余 1 个单位, 子节点和容器之间空余 1 个单位
```

`justify-content: space-around`
![[Pasted image 20241108112445.png]]
`justify-content: space-between`
![[Pasted image 20241108112637.png]]
`justify-content: space-evenly`
![[Pasted image 20241108112707.png]]

### margin
把 h3右边的元素直接推到 container 的最右边
```css
nav > h3 {
margin-right: auto;
}
```

Position-items
把左边最后一个元素放到右边去
```css
.class_name {
	margin-left: auto; 
}
```

### flex-grow flex-shrink flex-basis
- 如果container超过200px， 最多是200px宽。 如果container 小于200px， 元素会shrink
- flex-grow 决定当container变大的时候，元素是否一起变大。 如果两个元素 flex-grow 的值不同， 则表示变大速度不同. 0表示不变大
- flex-shrink 决定了元素缩小的速度。 0 表示不变小
```css
flex: 1 1 0
flex-basis: 200px 
flex-grow: 1;
flex-shrink: 0;
```


```css
.container > div {
	flex: 1; 全部铺平所有子元素. 占满容器
}

.container > .search {
	flex: 2; 选中的 search 元素会占满整个容器, 剩余的元素大小不变
}
```

### align-items
首先只有在 html和 body 一起定了 height = 100%之后, container 定义 height = 100% 才会纵向占满整个浏览器
```css
.container: {
	align-items: flex-start /* 左上角 */
}
```

align-self 找到 flex 容器的内的元素. 指定它的属性
```css
.logout {
	align-self: flex-start /* 移动到纵向的顶部 */
}
```
![[Pasted image 20241108124057.png]]

### order
元素逆序. 默认的order是0
```css
.container {
	border: 5px solid #ffcc5c;
	display: flex;
}

.item1 {
	order: 1;
}

.item2 {
	order: 0;
}

.item3 {
	order: -1;
}
```
![[Pasted image 20241109152237.png]]
### mediaquery
对不同尺寸的屏幕做样式调整
```css
@media all and (max-width: 600px) {
.container {
	flex-wrap: wrap; 在尺寸缩小时会wrap
}

.container > li {
	flex: 1 1 50%; 每个元素占用50%的行
}
```

# CSS Grid
 grid-template 定义整体布局
```css
.container {
	display: grid;
	grid-template-columns: 100px auto 100px; /* 3 列 2 行 */
	grid-template-rows: 50px 50px;
	grid-gap: 3px;
}
```

![[Pasted image 20241113212754.png]]

```css
.container {
	display: grid;
	grid-template-columns: 100px auto; /* 2 列 3 行 */
	grid-template-rows: 50px 50px 200px;
	grid-gap: 3px;
}
```
![[Pasted image 20241113213039.png]]

fraction
```css
grid-template-columns: repeat(3, 1fr); /* 3个 fraction */
grid-template-rows: repeat(2, 50px); /* == 50px 50 px */

/* 简写 行 / 列 */
grid-template: repeat(2, 50px) / repeat(3, 1fr);
```
grid-column 定义局部单个元素
```css
.header {
	grid-column-start: 1; /* 从第一列开始 */
	grid-column-end: 3; /* 到最后一列结束 */
}

/* 简写 */
.header {
	grid-column: 1 /3 ;
}

/* 占用两个 column */
.footer {
	grid-column: 1 / span 2;
}

/* 到最后一列 */
.footer {
	grid-column: 1 / -1;
}

.menu {
	grid-row: 1 / 3;  /* 从第一列开始, 到第二 2 列结束 */

}
```
grid-template-area
```css
.container {
	height: 100%;
	display: grid;
	grid-gap: 3px;
	grid-template-columns: repeat(12, 1fr);
	grid-template-rows: 40px auto 40px;
	grid-template-areas:
		". h h h h h h h h h h ."
		"m c c c c c c c c c c c"
		". f f f f f f f f f f .";
}

.header {
	grid-area: h;
}

.menu {
	grid-area: m;
}

.content {
	grid-area: c;
}

.footer {
	grid-area: f;
}

```

![[Pasted image 20241114073357.png]]
### auto-fix minmax
```css
.container {
	display: grid;
	grid-gap: 5px;
	/* 最小100px, 最大 1fr, 根据 container 大小自动填充 */
	grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
	/* 为了避免 implicit row, 定义无论多少行, 行高都是 100px */
	grid-auto-rows: 100px;
	/* 默认是 row 每个元素占用一行. dense 可以填充 */
	grid-auto-flow: dense;
}
```



```css
.container {
	height: 100%;
	display: grid;
	grid-gap: 3px;
	/* main-start 第一列的最左边分界线 */
	grid-template-columns: [main-start] 1fr [content-start] 5fr [content-end main-end];
	grid-template-rows: [main-start] 40px [content-start] auto [content-end] 40px [main-end];

}

.header {
	grid-column: main;
}
.menu {}
.content {
	grid-area: content;
}

.footer {
	grid-column: main;
}
```

![[Pasted image 20241114173736.png]]

### justify-content align-content
```css

justify-content: space-around;
align-content: center;
```
![[Pasted image 20241114174227.png]]
### justify-item align-item
对元素中的内容是否缩放元素
```css

justify-items: stretch; /* 默认是 stretch */
align-items: center;
```

### auto-fit auto-fill
如果容器很大, 如果元素不足, auto-fit会填充空间. auto-fill不会
![[Pasted image 20241114175422.png]]


grid case
```css
article {
	display: grid;
	grid-template-columns: 80px 1fr 80px; 分 3 列
}

article > * {
	grid-column: 2;
	min-width: 0; 防止内容过多 overflow 到第3 列

}
```
# Case
### ul tag
```css
ul {
	list-style: none;  /* 取消圆点 */
}

ul > li {
	padding-block: 17px; /* 扩大每个行之间的距离*/
}

```

### wrap

```css
.target {
	max-width: 400px; 
}
```