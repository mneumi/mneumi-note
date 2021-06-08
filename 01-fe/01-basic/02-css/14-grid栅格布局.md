## 布局方法一览（历史发展顺序）

1. 表格布局
2. 浮动+定位+盒子模型布局
3. flex布局
4. grid布局



## 栅格系统概述

### 类比

栅格系统类似于表格

### 不普及的原因

兼容性问题，现在大部分浏览器还不支持栅格系统

学习存在成本较大

### 对比 flex 布局

#### 一维还是二维布局

一维用flex，二维用grid

#### 从内容出发还是出布局出发

内容出发：先有内容（数量不固定）

布局出发：先规划网格（数量固定），再填充内容



## 定义栅格系统

### 1. 开启栅格布局

```css
display: grid;
width: 300px;
height: 300px;
```

### 2. 定义行与列

```css
/* 基础写法 */
gird-template-rows: 100px 200px 150px; /* 有3行，高度分别为100px 200px 150px */
grid-template-columns: 100px 100px 100px 100px; /* 有4列，宽度均为100px */

/* 百分比写法 */
grid-template-rows: 50% 50%; /* 有2行，高度为盒子高度一半 */
grid-template-columns: 20% 20% 20% 20% 20%; /* 有5列，宽度均为盒子宽度1/5 */

/* 重复简化写法 */
/* 相当于 grid-template-rows: 100px 100px 100px 100px 100px; */
grid-template-rows: repeat(5, 100px);

/* auto-fill：如果数量不确定时，可以使用auto-fill，将默认填充 */
grid-template-rows: repeat(auto-fill, 100px);

/* fr：按比例划分，类似于flex-grow */
grid-template-rows: repeat(3, 1fr); // 分为3份，每份占1/3
grid-template-rows: 1fr 2fr 1fr;
```

### 3. 栅格间隔

```css
/* 行间距 */
row-gap: 10px;

/* 列间距 */
column-gap: 20px;

/* 简写：gap: 行间距 列间距; */
gap: 10px 20px;
```

### 4. 栅格整体对齐方式

```css
justify-content: start | center | end | space-around | sapce-between | space-evenly | stretch
align-content: start | center | end | space-around | sapce-between | space-evenly | stretch
```

### 5. 栅格内元素对齐方式

```css
justify-items: start | center | end | space-around | sapce-between | space-evenly | stretch
align-items: start | center | end | space-around | sapce-between | space-evenly | stretch
```



## 部署栅格元素

### 1. 规划栅格区域

```css
/* 前提的栅格布局，即3行、3列 */
display: grid;
grid-template-rows: 6rem 1fr 6rem; 
grid-template-columns: 20rem 1fr 20rem;

/* 规划栅格区域 */
grid-template-areas: 
    "header header header"
    "nav main aside"
    "footer footer footer";
```

### 2. 设置元素

```css
.header {
    grid-area: header;
}

.main {
    grid-area: main;
}

.nav {
    grid-area: nav;
}

.aside {
    grid-area: aside;
}

.footer {
    grid-area: footer;
}
```



## 整体案例

### 概述

使用 CSS in JS 模式编写

### 示例代码

```tsx
// grid-area：用于给grid子元素起名字
const Header = styled.header`grid-area: header`
const Main = styled.main`grid-area: main`
const Nav = styled.nav`grid-area: nav`
const Aside = styled.aside`grid-area: aside`
const Footer = styled.footer`grid-area: footer`

const Container = styled.div`
	display: grid;
	grid-template-rows: 6rem 1fr 6rem;
	grid-template-columns: 20rem 1fr 20rem;
	grid-template-areas: 
		"header header header"
		"nav main aside"
		"footer footer footer";
	grid-gap: 10rem;
	height: 100vh;
`;
```

