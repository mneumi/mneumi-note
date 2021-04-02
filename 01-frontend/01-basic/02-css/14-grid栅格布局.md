## 布局方法一览（历史发展顺序）

1. 表格布局
2. 浮动+定位+盒子模型布局
3. flex布局
4. grid布局



## 栅格系统概述

#### 类比

栅格系统类似于表格

#### 不普及的原因

##### 兼容性问题

现在大部分浏览器还不支持栅格系统

##### 学习存在成本



## 栅格系统使用

#### 1.创建栅格系统容器

```css
display: grid;
width: 300px;
height: 300px;
```

#### 2.创建栅格-设置行列

```css
display: grid;
grid-template-rows: 100px 200px 150px; /* 有3行，高度分别为100px 200px 150px */
grid-template-columns: 100px 100px 100px 100px; /* 有4列，宽度均为100px */
```

百分比写法

```css
display: grid;
grid-template-rows: 50% 50%; /* 有2行，高度为盒子高度一半 */
grid-template-columns: 20% 20% 20% 20% 20%; /* 有5列，宽度均为盒子宽度1/5 */
```

#### 3.简化写法-重复

repeat(数量，长度)

```css
display: grid;

grid-template-rows: repeat(5, 100px);
/* 相当于 */
grid-template-rows: 100px 100px 100px 100px 100px;
```

```css
grid-template-rows: repeat(2, 100px 50px);
/* 相当于 */
grid-template-rows: 100px 50px 100px 50px;
```

auto-fill：如果数量不确定时，可以使用auto-fill，将默认填充

```css
grid-template-rows: repeat(auto-fill, 100px)
```

#### 4.按比例

fr：按比例划分，类似于flex-grow

```css
grid-template-rows: repeat(3, 1fr); // 分为3份，每份占1/3
```

```css
grid-template-rows: 1fr 2fr 1fr;
```

#### 5.按范围（最小最大值）

minmax

```css
grid-template-rows: repeat(2, minmax(50px, 100px));
```

#### 6.栅格间距

行间距

```css
row-gap: 10px;
```

列间距

```css
column-gap: 20px;
```

简写

```css
gap: 行间距 列间距;
```

#### 7.栅格流动机制

```css
grid-auto-flow: row | column [dense];
```

#### 8.栅格整体对齐方式

```css
justify-content: start | center | end | space-around | sapce-between | space-evenly | stretch
align-content: start | center | end | space-around | sapce-between | space-evenly | stretch
```

#### 9.栅格内元素对齐方式

```css
justify-items: start | center | end | space-around | sapce-between | space-evenly | stretch
align-items: start | center | end | space-around | sapce-between | space-evenly | stretch
```

#### 10.栅格元素的独立控制对齐

```css
justify-self: start | center | end | space-around | sapce-between | space-evenly | stretch
align-self: start | center | end | space-around | sapce-between | space-evenly | stretch
```

#### 11.栅格对齐组合写法

```css
place-content: [align-content] [justify-content]
place-items: [align-items] [justify-items]
place-self: [align-self] [justify-self]
```







## 部署元素——使用边线

#### 编号形式——必须是矩形

```css
grid-row-start: 1; // 从第一行开始
grid-column-start: 1; // 从第一列开始
grid-row-end: 2; // 结束于第二列
grid-column-end: 4; // 结束于第四列

------------------
|  xx | xx  | xx |
------------------
|     |     |    |
------------------
|     |     |    |
------------------
```

##### 简写

```css
grid-row: 1/2;
grid-column: 1/4;

效果相当于

grid-row-start: 1;
grid-row-end: 2;
grid-column-start: 1;
grid-column-end: 4;
```

```css
grid-row: 1/span 3;
grid-column: 1/span 3;

效果相当于

grid-row-start: 1;
grid-row-end: span 3;
grid-column-start: 1;
grid-column-end: span 3;
```

#### 命名形式

可以为栅格命名

```css
grid-template-rows: [r1-start] 100px [r1-end r2-start] 100px [r2-end r3-start] 100px [r3-end];
grid-template-columns: [c1-start] 100px [c1-end c2-start] 100px [c2-end c3-start] 100px [c3-end];
```

使用栅格名称放置元素

```css
grid-row-start: r1-start;
grid-column-start: c1-end; /* c2-start */
grid-row-end: r1-end; /* r2-start */
grid-column-end: c2-end; /* c3-start */
```

##### 重复命名

```css
grid-template-rows: repeat(3, [r-start] 1tr [r-end])
```

```css
grid-row-start: r-start 1;
grid-row-end: r-end 2;
grid-column-start: c-start 1;
grid-column-end: c-end 2;
```

#### 偏移量形式

先用grid-row-start和grid-column-start定义开始位置后，再使用偏移量表明占用的空间

```css
grid-row-start: 2;
grid-column-start: span 2;
grid-column-end: span 3;
```



## 开发类似Bootstrap的栅格系统

```css
.container {
    width： 1200px;
    margin: 0 auto;
}

.row {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
}

.col-1 {
    grid-column-end: span 1;
}
.col-2 {
    grid-column-end: span 2;
}
.col-3 {
    grid-column-end: span 3;
}
.col-4 {
    grid-column-end: span 4;
}
.col-5 {
    grid-column-end: span 5;
}
.col-6 {
    grid-column-end: span 6;
}
.col-7 {
    grid-column-end: span 7;
}
.col-8 {
    grid-column-end: span 8
}
.col-9 {
    grid-column-end: span 9;
}
.col-10 {
    grid-column-end: span 10;
}
.col-11 {
    grid-column-end: span 11;
}
.col-12 {
    grid-column-end: span 12;
}
```



## 部署元素——使用区域

#### 根据线

```css
grid-area: 1/1/2/4; /* grid-row-start/grid-column-start/grid-row-end/grid-column-end */
```

```css
{
    grid-template-rows: repeat(3, [r] 1fr);
    grid-template-columns: repeat(3, [c] 1fr);
}

{
    grid-area: r 2/c 2/r 3/c 3;
}
```

#### 根据名称

```css
{
    grid-template-rows: 60px 1fr 60px;
    grid-template-columns: 60px 1fr;
    grid-template-areas: "header header"
        "nav main"
        "footer footer";
}

{
    grid-area: header-start/header-start/main-end/main-end;
}
```



================================



## https://www.bilibili.com/video/bv1Q54y1272g
