


# Position

# Relative & Absolute
默认情况下， 所有标签都有`position:static`; 属性。如果是文本元素，就是从左往右排列。如果是行级元素就是从上到下排列


* 如果要配置某个子元素为absolute脱离文档流, 则需要把父标签设置为relative
* 通常情况下relative不会和top, right, bottom连用
* 如果某个元素配置了absolute, 则后面的元则会当这个元素不存在
  被设置为aboslute的元素, 只会参照距离自己最近的一个position被配置为relative的元素, 然后与top, left, right, bottom连用确定自己的问题. 如果上层没有一个元素position设置为relative, 则会对标整个页面

position是完全脱离文档流
块级标签本身独占一行, 靠文档左侧对齐
position属性


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