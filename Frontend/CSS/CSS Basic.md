https://www.bilibili.com/video/BV1p84y1P7Z5?p=9&spm_id_from=pageDriver&vd_source=708ab8355a6d2b20c5a1edf85bb4a577

# 进度
P1-P9 
P57

感悟： HTML的键值对是用等号的， CSS的键值对是用冒号的


### refer css
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

## 最佳实践

默认头部配置
本质上，root标签是html， 然后里面有个body标签，但是body标签是有默认margin的。所以下面这个全局配置，会先把所有css对象的margin和情况
```css

* {
    margin: 0;
    padding: 0;
     /* 如果长宽给定, 则配置padding不会使 box变大， 而是压缩内容*/
    box-sizing: border-box; 
}
```



### Unit
- 绝对长度 px
- 相对长途 % em
	- 2em 表示2倍

### Box Model

默认情况下, padding是零, board是零
顺时针, 如果对应位置为空, 在对应位置对面方向的值
```css

height: 140px; /*内容的高, 不是盒子的尺寸*/
width:  140px; /*内容的宽*/

border: solid 3px green; /*虚线dashed 电线 dotted*/

border-top-color: black; /*顶部边框的颜色*/

border-top-width: 10px; /* 底部 border-bottom*/

padding-top: 10px 20px 10px;  /* 上面 左右 下面 */

margin: 10px 20px 10px; /*本元素和其他元素的间隔, 或者浏览器的距离*/

margin: 10px auto 10px; /*左右对齐*/

```


```css

margin: 10px 20px 20px 10px; /* top right bottom left */

margin: 10px 20px /* top-bottom left-right */

margin: 10px /* all 4 direction*/

margin: 10px 20px 10px;  /* 上面 左右 下面 */

```

  

注意1:
当设置height和width时没有把margin和padding考虑在内的, 调整margin不会改变盒子的大小, 但是调整padding时会把盒子变大

注意2:
padding是通过扩充box的尺寸来实现的

注意3: 上下塌陷
平行两个box之间的margin是叠加的
上下两个box之间的margin是取上下两个margin中大的一个

HSL参数, 容易修改颜色亮度和饱和度

1st: degree of the hue, and can be between 0 and 360. 2nd: second and third numbers are percentages representing saturation

3rd: lightness respectively.

  

#常用样式

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

```

### text

```css

font-family: Helvetica, Arial, sans-serif; /*按照选择次序排列*/

line-height:100px; /*一行的高度, 文本上下居中*/

vertical-align: -4x; /*从底部向上垂直调整*/

word-spacing:10em; /*单词间距, 对基本标签同样适用*/

word-spacing:-10em; /*没有间距, 或者重叠*/

letter-spacing: 10em; /*字母间距*/

text-transform: capitalize


```

  

列表属性

```css

list-style:none; /*在布局中使用*/

list-style: circle/decimal/disc/upper-alpha;

```

### display

```css

display:none; /*不显示, 元素不占位置*/

display:block; /*取消隐藏, 将标签设置为块标签, 可以调整高度和宽度, 但是独占一行*/

display:inline; /*将标签设为内联标签, 不占一行*/

display:inline-block; /*一行显示多个元素, 且每个元素可以调整大小*/
```

  

例子:
```css

span {

  display: inline-block;

  height: 30px;

  width: 20px;

}
```

滚动条:
```css

overflow: hidden; /*隐藏超出边界的内容*/

overflow: auto; /*有滚动条*/

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

  
  
  

### CSS Selector
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
## Flexbox
- 首先flex 需要一个 container, 这个 container 下面一级的元素设置 `display: flex`, 会把这些元素放在一行里. 注意这个只作用于一级
- 默认情况下, main axis 是横向的, 从左往右. cross axis 是纵向的, 从上往下. 通过 `flex-direction: row` 默认来控制

justify content 布局
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