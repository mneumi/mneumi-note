## BFC简介

### BFC是什么

BFC全称 Block Formatting Context，直译为块级格式化上下文

### BFC作用

BFC是一种独立的环境，起到隔离保护的作用，让BFC中的元素与外部元素隔离起来，让BFC中元素不会影响到外部的布局



## 触发BFC的方法

1.使用绝对定位或使用fixed定位

2.使用overflow属性（只要不是`visible`这个值都可以，即`hidden`, `auto`, `scroll` 都可以）

3.使用浮动float，值不能是`none`，即`left`或`right`都可以

4.使用display属性，值为 `inline-block`，`block`, `table-cell` 或者 `flex`



## BFC能够解决什么问题

### 解决浮动造成父元素高度坍塌问题

设置父元素的`overflow: hiden`

### 解决上下外边距重合问题

设置父元素的`overflow: hiden`