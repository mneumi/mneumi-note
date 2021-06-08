# SCSS



## 初识SCSS

### SCSS介绍

SCSS 是一款强化 CSS 的预处理器，它在 CSS 语法的基础上增加了变量、嵌套 、混合、导入等高级功能，这些拓展令 CSS 更加强大与优雅

### SASS与SCSS区别

SCSS有两种语法格式，分别是：SCSS和SASS

#### SASS

SASS是最初的版本，文件扩展名为`.sass`

SASS采用缩进而不是`{}`与`;`的描述格式，通常，在 CSS 或 SCSS 中书写花括号时， 对应着在缩进语法中再缩进一层即可，任何一行结束， 都被当作分号

在缩进语法中还有一些额外的差异

#### SCSS

SCSS是SASS的升级版本，文件扩展名为`.scss`

SCSS是在 CSS3 语法的基础上进行拓展，它是 CSS 的超集，所有 CSS3 语法在 SCSS 中都是通用的，同时加入 Sass 的特色功能

#### 总结

SCSS写起像CSS，而SASS写起来不像CSS，推荐使用SCSS



## 指定字符集

### 作用

如果想在SCSS中使用中文或其他非ASCII字符，则需要指定SCSS文件的字符集

### 语法

```scss
@charset "UTF-8";
```



## 注释

### 注释方式

SCSS支持两种注释方式：`/* 注释 */` 和 `//`

#### 注释区别

非压缩模式下，`/* 注释 */`会被完整输出到编译后的 CSS 文件中，而 `//` 不会输出到编译后的 CSS 文件中

示例：

```scss
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }
```

编译后

```css
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; }

a {
  color: green; }
```

#### 强制输出注释

压缩模式下，两种注释都会被删除，但如果想不被删除，则可以使用`!`配合`/**/`使用

示例

```scss
@charset "UTF-8"; // 使用了中文内容，需要指定字符集
/*
  这是多行注释
*/

// 这是单行注释

/*! 强制输出的注释内容*/

.alert {
    padding: 15px;
}
```

编译后为

```css
@charset "UTF-8"; // 使用了中文内容，需要指定字符集

/*! 强制输出的注释内容*/

.alert {
    padding: 15px;
}
```



## 变量

### 定义方式

```scss
$变量名: 变量值;
```

### 变量名规则

支持字母，数字，下划线和`-`

```scss
$primary-color: #1269b5;
$primary_border: 1px solid $primary-color;
```

### 变量值类型

变量值支持类型如下 字符串，数字，布尔值，颜色，列表，null值

| 类型   | 示例                                          |
| ------ | --------------------------------------------- |
| 字符串 | "foo", 'bar', baz  `支持无引号`               |
| 数字   | 1, 2, 13, 10px                                |
| 布尔值 | true，false                                   |
| 颜色   | blue, #04a3f9, rgba(255,0,0,0.5)              |
| 列表   | 1.5em 1em 0 2em, Helvetica, Arial, sans-serif |
| 对象   | (key1: value1, key2: value2)                  |
| 空值   | null                                          |

### 使用变量

直接使用即可

```scss
#main {
  width: $width;
}
```

### 块级作用域

变量支持块级作用域，嵌套规则内定义的变量只能在嵌套规则内使用（局部变量），不在嵌套规则内定义的变量则可在任何地方使用（全局变量）

```scss
#main {
  $width: 5em;
  width: $width; // 只能在 #main 中使用
}
```

编译为

```css
#main {
  width: 5em;
}
```

将局部变量转换为全局变量可以添加 `!global` 声明

```scss
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```

编译为

```css
#main {
  width: 5em;
}

#sidebar {
  width: 5em;
}
```



## 嵌套

### 作用

SCSS支持嵌套写法从而简化css中过多的重复，如

```css
.nav {
    height: 100px;
}

.nav ul {
    margin: 0;
}

.nav ul li {
    float: left;
    list-style: none;
    padding: 5px;
}
```

可以使用SCSS简写为

```scss
.nav {
    height: 100px;
    ul {
        margin: 0;
        li {
            float: left;
            list-style: none;
            padding: 5px;
        }
    }
}
```

### 嵌套时获取父选择器

语法：使用`&`

使用场景：使用伪类或伪元素时

```scss
a {
    display: block;
    color: #000;
    padding: 5px;
    &:hover {
        background-color: #0d2f7e;
        color: #fff;
    }
}
```

编译后为

```css
a {
    display: block;
    color: #000;
    padding: 5px;
}

a:hover {
    background-color: #0d2f7e;
    color: #fff;
}
```

原理：使用了`&`的地方会被替换为父选择器

```scss
.nav {
	height: 100px;
    & &-text {
        font-size: 15px;
    }
}
```

编译后为

```css
.nav {
    height: 100px;
}

.nav .nav-text {
    font-size: 15px;
}
```

### 属性嵌套

对于具有相同前缀的属性，可以使用属性嵌套

```css
body {
    font-family: "Arial";
    font-size: 15px;
    font-weight: normal;
}

.nav {
    border: 1px solid #000;
    border-left: 0;
    border-right: 0;
}
```

可以改写为

```scss
body {
    font: {
        family: "Arial";
        size: 15px;
        weight: normal;
    }
}

.nav {
    border: 1px solid #000 {
    	left: 0;
        right: 0;
    }
}
```



## Mixin（混合）

### 作用

Mixin可以理解为一块定义好的样式，可以在任意的地方复用它

### 定义Mixin

#### 语法

```scss
// 带参数
@mixin 名字(参数1, 参数2, ...) {
	...
}

// 无参数
@mixin 名字 {
	...
}
```

#### 示例

```scss
@mixin alert {
    color: #8a6d3b;
    background: #fcf8e3;
    a {
        color: #8a6d3b;
    }
}
```

### 使用Mixin

#### 语法

```scss
@include Mixin名称
```

#### 示例

```scss
.alert-warning {
    @include alert;
}
```

编译后为

```css
.alert-warning {
    color: #8a6d3b;
    background: #fcf8e3;  
}

.alert-warning a {
    color: #8a6d3b;
}
```

### 示例有参Mixin

#### 定义

```scss
@mixin alert($text-color, $background) {
    color: $text-color;
    background: $background;
    a {
        color: darken($text-color, 10%);
    }
}
```

#### 使用

```scss
.alert-warning {
    @include alert(#8a6d3b, #fcf8e3);
}
```

编译后为

```scss
@mixin alert($text-color, $background) {
    color: #8a6d3b;
    background: #fcf8e3;
    a {
        color: #664c2b;
    }
}
```



## 继承

### 作用

让一个选择器去继承另外一个选择器中所有（包括后代）的样式

### 语法

```scss
@extend 选择器;
```

### 示例

```scss
.alert {
    padding: 15px;
}

.alert a {
    font-weight: bold;
}

.alert-info {
    @extend .alert;
    background-color: #d9edf7;
}
```

编译后为

```css
.alert, .alert-info {
    padding: 15px;
}

.alert a, .alert-info a {
    font-weight: bold;
}

.alert-info {
    background-color: #d9edf7;
}
```



## Partials

### 作用

CSS自带有带入语句`@import`，这个语句有个缺点就是：每次遇到`@import`都会重新发送一个`HTTP`请求去获取它

而SCSS能够将样式放置于不同的小文件中，在编译时将它们融合起来，而不会将它们编译为CSS，这些小文件称为`Partials`

### 语法

Partial文件的名称需要以`_`开头

### 示例

_base.scss文件

```scss
body {
    padding: 0;
    margin: 0;
}
```

style.scss文件

```scss
@import "base";

.alert {
    padding: 15px;
}

.alert a {
    font-weight: bold;
}

.alert-info {
    @extend .alert;
    background: #d9edf7;
}
```

编译后为

style.css文件（只有这个文件，没有_base.css）

```css
body {
    margin: 0;
    padding: 0;
}

.alert, .alert-info {
    padding: 15px;
}

.alert a, .alert-info a {
    font-weight: bold;
}

.alert-info {
    background-color: #d9edf7;
}
```



## 数据类型

### 分类

SCSS中数据类型可以分为下表

变量值支持类型如下 字符串，数字，布尔值，颜色，列表，null值

| 类型   | 示例                                          |
| ------ | --------------------------------------------- |
| 字符串 | "foo", 'bar', baz  `支持无引号`               |
| 数字   | 1, 2, 13, 10px                                |
| 布尔值 | true，false                                   |
| 颜色   | blue, #04a3f9, rgba(255,0,0,0.5)              |
| 列表   | 1.5em 1em 0 2em, Helvetica, Arial, sans-serif |
| 键值对 | (key1: value1, key2: value2)                  |
| 空值   | null                                          |

### Boolean

#### 特点

值只有 true 或者 false

可用于@if，@for, @while指令

#### 运算

支持 and，or，not三个逻辑运算

```scss
(5px > 3px) and (5px > 10px); // false
(5px > 3px) and (5px < 10px); // true

(5px > 3px) or (5px > 10px); // true
```

### Number

#### 特点

SCSS中的数字可以带单位，如 `15px`，`30%`，`80rem` 等等

#### 数字运算

```scss
2 + 8; // 10
2 * 8; // 16

8 / 2; // 8/2
(8 / 2); // 4

5px + 5px; // 10px;
5px - 2; // 3px;
5px * 2; // 10px;

(10px / 2px); // 5
(10px / 2); // 5px
```

### String

#### 特点

SCSS中的字符串支持双引号，单引号以及无引号

无引号的区别就是无法携带空格，类比于下列的css代码

```css
font-family: Courier, "Lucida Console", 'monspace' 
```

#### 运算

用`+`进行字符串拼接

```scss
"Hello " + World; // Hello World
```

注意点：第一个字符串的引号决定输出的字符串的引号

| 写法               | 输出值        |
| ------------------ | ------------- |
| "Hello " + "World" | "Hello World" |
| "Hello " + 'World' | "Hello World" |
| "Hello " + World   | "Hello World" |
| 'Hello ' + "World" | 'Hello World' |
| 'Hello ' + 'World' | 'Hello World' |
| 'Hello ' + World   | 'Hello World' |
| Hello + "World"    | HelloWorld    |
| Hello + 'World'    | HelloWorld    |
| Hello + World      | HelloWorld    |

### Color

支持三种写法：

* 字符串：`red`，`blue`
* rgb(a)：rgb(255, 10, 0)，rgba(255, 10, 0, 0.8)
* 十六进制：#ff6666，#66ccff

### List

#### 特点

分隔可以使用空格或者逗号，类比于下列的css

```css
border: 1px solid #000;
font-family: Courier, "Lucida Console", monspace;
```

### Map

#### 定义语法

```scss
$m: (key1: value1, key2: value2,)
```



## 函数

### 数字函数

| 数字函数   | 说明     | 例子                       |
| ---------- | -------- | -------------------------- |
| abs        | 绝对值   | abs(-10px)                 |
| round      | 四舍五入 | round(3.14)                |
| ceil       | 向上取整 | ceil(3.14)                 |
| floor      | 向下取整 | floor(3.14)                |
| percentage | 取百分比 | percentage(650px / 1000px) |
| max        | 取最大值 | max(1,2,3)                 |
| min        | 取最小值 | min(1,2,3)                 |

### 字符串函数

```scss
$greeting: "Hello World";
```

| 字符串函数    | 说明       | 例子                              |
| ------------- | ---------- | --------------------------------- |
| to-upper-case | 转全大写   | to-upper-case($greeting)          |
| to-lower-case | 转全小写   | to-lower-case($greeting)          |
| str-length    | 计算长度   | str-length($greeting)             |
| str-index     | 查找索引   | str-index($greeting, "Hello")     |
| str-insert    | 插入字符串 | str-insert($greeting, ".net", 14) |

### 颜色函数

| 内置函数       | 说明                                         | 例子                                |
| -------------- | -------------------------------------------- | ----------------------------------- |
| rgb            | 红绿蓝；输出值为16进制或者字符串颜色         | rgb(255, 10, 0)                     |
| rgba           | 红绿蓝透明度；输出值为rgba                   | rgb(255, 10, 0, 0.8)                |
| hsl            | 色相饱和度明度；输出值为16进制或者字符串颜色 | hsl(60, 100%, 100%)                 |
| hsla           | 色相饱和度明度透明度；输出值为rgba           | hsl(60, 100%, 100%, 0.8)            |
| adjust-hue     | 调整颜色色相的值                             | adjust-hue($base-color-hsl, 137deg) |
| darken         | 颜色加深（加深明度）                         | darken(#ff6666, 10%)                |
| lighten        | 颜色变浅（减少明度）                         | lighten(#66ccff, 30%)               |
| saturate       | 加深颜色饱和度                               | saturate(#ff6666, 30%)              |
| desaturate     | 减少颜色饱和度                               | desaturate(#66ccff, 80%)            |
| opacify        | 增加颜色颜色不透明度                         | opacify(#66ccff, 0.3)               |
| transparentize | 减少颜色颜色不透明度                         | transparentize(#ff6666, 0.2)        |

### 列表函数

| 列表函数 | 说明                   | 例子                         |
| -------- | ---------------------- | ---------------------------- |
| length   | 获得列表的长度         | length(50px, 100px)          |
| index    | 获取元素在列表中的下标 | length(1px solid red, solid) |
| append   | 列表追加新元素         | append(5px 10px, 7px)        |
| join     | 融合两个列表           | join(5px 10px, 1px, 2px)     |

### 键值对函数

```scss
$colors: (light: #ffffff, dark: #000000);
```

| 键值对函数  | 说明                   | 例子                                |
| ----------- | ---------------------- | ----------------------------------- |
| length      | 获得键值对的元素数量   | length($colors)                     |
| map-get     | 由键获取值             | map-get($colors, dark)              |
| map-set     | 设置键值对             | map-set($colors, aya, #ff6666)      |
| map-remove  | 删除键值对             | map-remove($colors, light, dark)    |
| map-keys    | 获取所有键组成的列表   | map-keys($colors)                   |
| map-values  | 获取所有值组成的列表   | map-values($colors)                 |
| map-has-key | 判断键值对中是否含有键 | map-has-key($colors, light)         |
| map-merge   | 合并两个map            | map-merge($colors, (gray: #e5e5e5)) |



### 自定义函数

#### 语法

```scss
@function 名称(参数1, 参数2, ... ) {
    ...
    @return 返回值;
}
```

#### 示例

```scss
$ratio: 375 / 100;

@function p2r($px) {
    @return $px / $ratio + rem;
}
```



## 插值

### 作用

让一个值插入到另外一个值中，类似于ES6的插值表达式

### 语法

```scss
#{值}
```

### 示例

```scss
$version: "0.0.1";

/*! 当前版本号是 #{version} */

$name: "info";
$attr: "border";

.alert-#{$name} {
    #{$attr}-color: #ccc;
}
```

编译后为

```css
@charset "UTF-8";
/* 当前版本号是 0.0.1 */
.alert-info {
    border-color: #ccc;
}
```



## 控制指令

### @if @else-if @else

#### 作用

条件语句

#### 语法

```scss
@if 表达式 {
    ...
} @else if 表达式 {
    ...
} @else {
    ...
}
```

#### 示例

```
$use-prefixes: false;

@if $use-prefixes {
	color: red;
} @else {
	color: blue;
}
```

### @for

#### 作用

循环语句

#### 语法

```scss
@for $var from <开始值> through <结束值> { // 包含结束值
    ...
}

@for $var from <开始值> to <结束值> { // 不包含结束值
    ...
}
```

#### 示例1

```scss
$columns: 4;

@for $i from 1 through $columns {
    .col-#{$i} {
        width: 100% / $columns * $i;
    }
}
```

编译后为

```css
.col-1 {
    width: 25%;
}

.col-2 {
    width: 50%;
}

.col-3 {
    width: 75%;
}

.col-4 {
    width: 100%;
}
```

#### 示例2

```scss
$columns: 4;

@for $i from 1 to $columns {
    .col-#{$i} {
        width: 100% / $columns * $i;
    }
}
```

编译后为

```css
.col-1 {
    width: 25%;
}

.col-2 {
    width: 50%;
}

.col-3 {
    width: 75%;
}
```

### @each

#### 作用

用于遍历列表

#### 语法

```scss
@each $var in $list {
    ...
}
```

#### 示例

```scss
$icons: success error warning;

@each $icon in $icons {
    .icon-#{$icon} {
        background-image: url(../images/icons/#{$icon}.png)
    }
}
```

编译后为

```css
.icon-success {
  background-image: url(../images/icons/success.png);
}

.icon-error {
  background-image: url(../images/icons/error.png);
}

.icon-warning {
  background-image: url(../images/icons/warning.png);
}
```

### @while

#### 作用

循环语句

#### 语法

```scss
@while 条件 {
    ...
}
```

#### 例子

```scss
$i: 6;

@while $i > 0 {
    .item-#{$i} {
        width: 5px * $i;
    }
    $i: $i - 2;
}
```

编译后为

```css
.item-6 {
    width: 30px;
}

.item-4 {
    width: 20px;
}

.item-2 {
    width: 10px;
}
```



## 错误与警告指令

### 作用

可以在自定义函数或Mixin中自定义错误与警告，当错误使用函数或Mixin时，将会进行错误和警告

### 语法

```scss
@warn "警告信息";
@error "错误信息";
```

### 例子

```scss
$colors: (ligth: #ffffff, dark: #000000);

@function color($key) {
    @if not map-has-key($colors, $key) {
        @warn "在 $colors 中没有找到 #{$key}"
    }
    
    @return map-get($colors, $key);
}
```

```scss
$colors: (ligth: #ffffff, dark: #000000);

@function color($key) {
    @if not map-has-key($colors, $key) {
        @error "在 $colors 中没有找到 #{$key}"
    }
    
    @return map-get($colors, $key);
}
```



## 总结

### 变量

以`$`开头，可以包含`-`

### 插值

以`#{}`包裹

### 指令

指令都是以`@`开头的

| 指令               | 功能       | 例子                              |
| ------------------ | ---------- | --------------------------------- |
| @charset           | 指定字符集 | @charset "UTF-8";                 |
| @mixin             | 定义Mixin  | @mixin center {}                  |
| @include           | 使用Mixin  | @include center;                  |
| @extends           | 继承       | @extends .alert;                  |
| @function          | 定义函数   | @function p2r($px) {}             |
| @return            | 函数返回   | @return $px + rem;                |
| @if @else if @else | 条件指令   | @if map-has-key($colors, dark) {} |
| @for               | 循环指令   | @for $i from 1 to 10 {}           |
| @while             | 循环指令   | @while @i > 0 {}                  |
| @each              | 遍历列表   | @each $item in $list {}           |
| @warn              | 警告       | @warn "警告提示信息"              |
| @error             | 错误       | @warn "错误提示信息"              |

### SCSS中异于其他语言的点

1. 字符串可以不用引号
2. 数字类型可以带单位，如`px`，`em`，`rem`等
3. 列表类型可以用空格或逗号分隔，且两端无括号
4. 键值对类型两端使用`()`
5. 拥有强注释
6. 变量名支持`-`