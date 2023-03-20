https://www.bilibili.com/video/BV1p84y1P7Z5?p=9&spm_id_from=pageDriver&vd_source=708ab8355a6d2b20c5a1edf85bb4a577

# 进度
P1-P9 
P57


HTML的键值对是用等号的
CSS的键值对是用冒号的

## Best Practice

在css头部配置

```css

* {

    margin: 0;

    padding: 0;

    box-sizing: border-box; /* 如果长宽给定, 则配置padding不会使 box变大 */

}

```

问题： 这里长和宽是盒子模型的那个部分？

## selector

```css

input, button {} /* select from multiple tag */

.first {} /* select from class */

#id {} /* select from id */

#id h2 {} /* select h2 in div id */

```

  
  
  

# CSS Syntax

  

## 引入方式

  

CSS的引入方式分为3种

根据就近原则, 引入方式有优先级

1. 在html头部直接声明(很少使用, 耦合性太强
2. 行内引入, 优先级最高
3. 外部引入
```html
<link href="/path/style.css" rel="stylesheet" />
```

  

#### 直接声明

  

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

  

#### 行内引入

  

```css

<div style="front-size: 20px;color: blue">

</div>

```

  

#### 外部引入

  

```css

/* type属性默认是 text/css 可以省略*/

<link rel="stylesheet" href="stylesheet.css" type="text/css">

  

/*引入在线字体, 400, 700两种粗细样式 */

 <link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Space+Mono:400,700">

```

  

## 选择器selector

  

CSS有三种选择标签的方式, 分别是

  

1. 直接选择标签名字

2. 选择带有属性值 class 的标签

3. 选择带有属性值 id 的标签

4. 层级选择器

5. 属性选择器

  

#### 通用选择器

  

一般在css首行使用, 因为有些浏览器都有默认的css设置,

这个选择器对所有的css属性清零, 包括`<body>`的margin

  

```css

* {

  margin: 0;

  padding:0;

}

  

```

  

#### tag 选择器

  

因为在一个页面中, 多次使用相同的标签, 所以tag选择器的应用范围是整个页面中的同名标签.

  

```css

 h1 {

     color: green;

  }

```

  

#### class 选择器

  

class标签可以跨标签修饰, 即不同种类的标签如果有相同的class属性值, 可以通过一个class选择器同时修饰.

另一方面, 一个tag可以还有多个class属性值, 也就是说一个tag可以被多个class选择器所修饰

  

```html

<div class="box">

    这是box

</div>

  

<style type="text/css">

  .box {

      color: green;

      font-size: 20px;

    }

</style>

```

  

一个tag中可以放置多个class

  

```html

<!--当我们设置多个class属性时, 可以用空格隔开-->

<p class="highlight module right"></p>

  

<!--多个选择器修饰同一个标签-->

<style type="text/css">

    .right {

       text-align: right;

    }

  

    .module {

      background-color: red;

    }

</style>

```

  

#### id 选择器

  

因为在一个页面中, 每一个id值都是唯一的, 所以id选择器用于修饰单个tag内容

  

```css

#site-descrption {

  color: green

}

```

  

#### 层级选择器

  

空格分隔层级

  

```css

ul li {

  color: red

}

```

  

#### 并列选择器

  

用逗号分隔两个并列的对象

  

```css

#id, .classname {

  color: red;

}

```

  

#### 子代选择器

  

子代选择器和层级选择器的区别在于, 如果儿子一层有, 子代选择器就可以找到, 如果儿子一层没有, 但是孙子一层有, 子代选择器就找不到孙子一层

  

```css

.menu > li {

      display: inline; /*在一行中显示*/

      margin-left: 37.5px;

    }

```

  

#### 属性选择器

  

在选择表单中的元素时, 多使用属性选择器

同时也可以自己定义属性

  

```css

/*在input标签中type就是属性, 并且可以指定属性值*/

input[type="text"] {

  color:red;

}

  

/*我在<p>中自己定义一个属性 name="fan"*/

p[name="fanxiao"] {

  color: red;

}

  

/*当一个标签被多个属性值修饰时, 选在以fan开头的属性值*/

[class^="fan"]{}

/*取以xiao结尾的属性*/

[class$="xiao"]{}

/*在属性中有fan这个单词的可以选择出来*/

[class~="fan xiao"]{}

/*只要属性中有字符串fa出现就能选择出来*/

[class*="fa"]{}

```

  

#### 毗邻选择器

  

```css

/*选择和outer毗邻的一个p标签(下面一个)*/

#outer+p {

  color: red;

}

```

  

#### 伪类伪元素选择器

  

```css

/*这里a可以是类选择器, 也可以是标签*/

a:hover{} /*在鼠标悬浮的时候*/

a:link{} /*在没有访问的时候*/

a:active{} /*在点击连接的瞬间*/

a:visited{} /*访问过之后*/

/*标签中插入内容*/

p:after{

  content:"add content";

}

p:before{

  content: "add before"

}

```

  

例子: 鼠标悬浮变色效果

  

```html

<style type="text/css">

 .profile-action {

      display: block;

      width: 100%;

      height: 32px;

   /*当鼠标悬浮时的背景颜色*/

 .profile-action:hover {

      background-color: #00A5D2;

      color: white;

    }

</style>

  

<body>

  <button class="profile-action">Set yourself away</button>

</body>

  

```

  

### 常见属性

  

在下面的链接中搜寻 [CSS属性参考](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform)

颜色

  

```css

color:#FFFFFF;

color:rgb(255,255,255)

hsl(325, 50%, 50%)

```

  

HSL参数, 容易修改颜色亮度和饱和度

1st: degree of the hue, and can be between 0 and 360. 2nd: second and third numbers are percentages representing saturation

3rd: lightness respectively.

  

#常用样式

  

字体

  

```css

color: red; /*字体颜色*/

font-size: 20px; /*字体大小*/

font-weight: 200; /*粗体*/

line-height: 40px; /*行高, 如果行高=height可以垂直居中*/

font-family:"Times New Roman"; /*字体*/

font-style:italic; /*斜体, 正常为normal, */

text-indent: 40px; /*首行缩进*/

text-decoration: none; /*去除a标签的下划线*/

text-align: center; /*水平居中*/

font: normal 20px/40px 'Microsoft Yahei'; /*非斜体 大小/行高 字体 */

```

  

背景

  

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

  

文本

  

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

  

display显示和隐藏

  

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

  

# Relative & Absolute

  

* 如果要配置某个子元素为absolute脱离文档流, 则需要把父标签设置为relative

  

* 通常情况下relative不会和top, right, bottom连用

  

* 如果某个元素配置了absolute, 则后面的元则会当这个元素不存在

  

  被设置为aboslute的元素, 只会参照距离自己最近的一个position被配置为relative的元素, 然后与top, left, right, bottom连用确定自己的问题. 如果上层没有一个元素position设置为relative, 则会对标整个页面

  
  
  

position是完全脱离文档流

块级标签本身独占一行, 靠文档左侧对齐

position属性

`position:static`; 默认属性, 元素从左到右排列

  

```css

/*相对于static的缺省位置,相对于static的默认位置移动标签, 配置top, bottom, left, right属性值 */

position: relative;

top: 20px;

left: 20px;

```

  

```css

/*浮动, 文档流中的其他标签会忽略设置了absolute属性的标签*/

/*本标签会移动到最近一个设置了position属性的父级标签的相对位置*/

/*和fixed属性值的区别在于: 滚动屏幕时标签会消失*/

position: absolute;

top:20px;

left:20px;

```

  

`position: fixed;` 滚动屏幕, 标签在窗口相对的位置不变

z-index:10 当设置了fixed之后, 在滚动屏幕时如果遇到了其他设置了position属性值的标签, 会被其遮挡, 这时需要设置z-index.

z-index类似于权重, 设置了z-index属性的元素会优先显示

  

### CSS Length

  

绝对长度 px

相对长途 % em

2em 表示2倍

  
  

```css

padding-top: calc(475px * 0.40);

margin: 0px auto 10px; /*top left-right button*/

margin: auto;

margin-top: 8px;

```

  
  

#### CSS Global Values

  

```css

 font-family: inherit;

```

  
  
  

### CSS 选择器优先级计算

  

`!important` 全局最优先

`#id` 优先级是

`.class` 优先级是

  

host在本地的字符集

[Free Fonts! Legit Free & Quality » Font Squirrel](https://www.fontsquirrel.com/)

  

@font-face {

  font-family: "Roboto";

  src: url("..font/Robot.woff") format('woff2'),

​       url("..font/Robot.woff") format('woff'),

​       url("..font/Robot.woff") format('truetype');

}

  

盒子模型

  

#### 盒子模型

  

![标准盒模型](CSS.assets/标准盒模型.png)

  

盒子的宽 = width + padding + border

  

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

  

### Margin & Padding

  

 默认情况下, padding是零, board是零

  

顺时针, 如果对应位置为空, 在对应位置对面方向的值

  

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

  

# 在线工具

  

[unminifier](https://mrcoles.com/blog/css-unminify/)
