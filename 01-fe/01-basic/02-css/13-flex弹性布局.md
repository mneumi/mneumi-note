## 介绍

### 作用

flex布局是CSS3为了布局而设计推出的一种技术

### 核心术语

弹性盒子，弹性元素，主轴，交叉轴



## 主轴与交叉轴

### 主轴

作用：弹性元素的排布方向就是主轴方向

方向：存在4种情况，①水平从左向右方向（默认）②垂直由上到下方向 ③水平从右向左 ④垂直由下到上

### 交叉轴

方向：交叉轴始终垂直于主轴

### 转换

主轴和侧轴并不是固定不变的，通过flex-direction可以互换



## 弹性盒子

#### 定义弹性盒子 【flex】

```css
display: flex;
```

#### 设置弹性盒子的主轴方向【flex-direction】

```css
flex-direction: row;  /* 默认值：水平排列 */
flex-direction: row-reverse;  /* 水平反向排列 */
flex-direction: column; /* 垂直排列 */
flex-direction: column-reverse; /* 垂直反向排列 */
```

#### 设置弹性盒子溢出情况【flex-wrap】

当子项目内容总宽度大于父盒子的时候是否进行换行

```css
flex-wrap: wrap(换行) | wrap-reverse | nowrap(默认)
```

| 值           | 描述                                           |
| ------------ | ---------------------------------------------- |
| nowrap       | 不换行，压缩显示，强制一行内显示完全（默认值） |
| wrap         | 换行                                           |
| wrap-reverse | 换行并且反向                                   |

#### 同时设置主轴方向和溢出情况【flex-flow】

```css
div {
  display: flex;
  flex-flow: row wrap;
  /*
  相当于
  flex-direction: row;
  flex-wrap: wrap;
  */
}
```

#### 主轴元素排布方式【justify-content】

```css
justify-content: flex-start(默认) | center | flex-end | space-between | space-around | space-evenly | stretch
```

| 值            | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| flex-start    | 按主轴方向，紧挨着                                           |
| flex-end      | 按主轴相反方向，紧挨着                                       |
| center        | 居中，紧挨着                                                 |
| space-between | 按主轴方向，相邻元素间距离相同；每行第一个元素与行首对齐，每行最后一个元素与行尾对齐 |
| space-around  | 按主轴方向，相邻元素间距离相同；每行第一个元素到行首的距离和每行最后一个元素到行尾的距离将会是相邻元素之间距离的一半 |
| space-evenly  | 按主轴方向，平均分布，相邻元素之间的间距 = 行首到第一个元素间距 = 最后一个元素到行尾间距 |
| stretch       | 拉伸，前提是不设置宽度/高度                                  |

#### 交叉轴单行元素排序方式【align-items】

```css
align-items: flex-start(默认) | center | flex-end | space-between | space-around | space-evenly | stretch
```

| 值         | 描述                                             | 白话文                                                |
| ---------- | ------------------------------------------------ | ----------------------------------------------------- |
| stretch    | 项目被拉伸以适应容器（没有设置高度时）（默认值） | 让子元素的高度拉伸适用父容器（子元素不给高度的前提下) |
| flex-start | 项目位于容器的开头                               | 垂直对齐开始位置上对齐                                |
| flex-end   | 项目位于容器的结尾                               | 垂直对齐结束位置底对齐                                |
| center     | 项目位于容器的中心                               | 垂直居中                                              |

#### 交叉轴多行元素排序方式【align-content】

```css
align-content: flex-start | center | flex-end | space-between | space-around | space-evenly;
```

前提条件

* 主轴为水平方向，即flex-direaction: row|row-reverse;
* 设置换行显示，即flex-wrap: wrap;
* 产生换行情况，即子项目总宽度大于父盒子宽度

| 值            | 描述                                             |
| ------------- | ------------------------------------------------ |
| stretch       | 项目被拉伸以适应容器（没有设置高度时）（默认值） |
| flex-start    | 项目位于容器的开头                               |
| flex-end      | 项目位于容器的结尾                               |
| center        | 项目位于容器的中心                               |
| space-between | 项目位于各行之间留有空白的容器内                 |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内   |

#### 交叉轴中特定元素控制【align-self】

```css
align-self: flex-start | center | flex-end | space-between | space-around | space-evenly | stretch;
```



## 弹性元素

#### 弹性盒子空间有剩余时分配【flex-grow】

当弹性盒子中有剩余空间时，可以将剩余空间分配给弹性元素

```css
flex-glow: 0 1 2 ... n
```

【数字越大，分配的额外空间越多】

flex-glow: 0 表示占剩余空间0份，即不用给它分配剩余空间

flex-glow: 1 表示占剩余空间1份

flex-glow: n 表示占剩余空间n份

总份数是 1+2+...+n

#### 弹性盒子空间不足时分配【flex-shrink】

【前提不换行：flex-wrap: nowrap或不设置】

当弹性盒子中空间不足时，规划每个弹性元素的缩小程度

```css
flex-shrink: 0 1 2 ... n
```

【数字越大，缩小程度越大】

flex: 0 表示不缩小，原来多大就多大

flex: 1 表示缩小1份

#### 弹性元素基准尺寸【flex-basis】

flex-basis决定主轴方向上元素的基准大小

当主轴为水平时，flex-basis相当于 width

当主轴为垂直时，flex-basis相当于 height

优先级情况：

* max/min-width > flex-basis > width
* max/min-height > flex-basis > height

#### 弹性元素的组合定义【flex】

flex-grow和flex-shrink和flex-basis可以同时定义

```css
flex: 1 2 100px
/* 相当于 */
flex-grow: 1;
flex-shrink 2;
flex-basis: 100px;
```

#### 弹性元素的顺序【order】

用整数值来定义排列顺序，数值小的排在前面

可以为负值，默认值是 0

```css
order: 1 /* 可以是任意数字，包括负数 */
```



## 注意事项

#### flex布局会将块内元素转为块元素

有类似效果的还有：position: fixed 和 position: absolute

#### 弹性布局对文本节点(innerHTML)也是有效果的

```html
<style>
  div {
    height: 100px;
    width: 100px;
    background-color: red;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
<div>
  我想居中啊
</div>
```

