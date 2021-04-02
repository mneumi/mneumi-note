## 概述

### CSS与组件化

现在整个前端已经是组件化的天下，但CSS在设计之初并不是为组件化而生的，CSS没有模块的概念，只有全局的层叠性，所以在目前组件化的框架中都在需要一种合适的CSS解决方案

组件化中合适的CSS解决方案应该符合以下条件

* 可以编写局部CSS：CSS具备自己的局部作用域，不会随意污染其他组件内的原生
* 可以编写动态的CSS：可以获取当前组件的一些状态，根据状态的变化生成不同的CSS样式
* 支持所有的CSS特性：伪类、动画、媒体查询等
* 编写起来简洁方便、最好能够符合CSS的命名风格

### Vue中的CSS

在CSS处理方面，Vue做的要好于React

* Vue通过在 `.vue` 文件中编写 `<style><style>` 标签来编写自己的样式
* 通过是否添加 scoped 属性来决定编写的样式是全局有效还是局部有效
* 通过 lang 属性来设置 less、sass 等预处理器
* 通过内联样式风格的方式来根据最新状态设置和改变CSS

### React中的CSS

相比而言，React官方并没有给出在React中统一的样式风格，所以出现4种流行的解决方案

* 内联样式
* 普通CSS
* CSS Module
* CSS in JS



## 方案1：内联样式

### 概述

内联样式是官方推荐的一种CSS样式的写法，style 属性接受一个采用小驼峰命名属性的 JavaScript 对象，而不是 CSS 字符串

### 优点与缺点

| 优点                          | 缺点                                   |
| ----------------------------- | -------------------------------------- |
| 内联样式, 样式之间不会有冲突  | 写法上都需要使用驼峰标识               |
| 可以动态获取当前state中的状态 | 某些样式没有代码智能提示               |
|                               | 大量的样式，容易造成代码混乱，难以维护 |
|                               | 某些样式无法编写(比如伪类/伪元素)      |

### 示例代码

```jsx
render() {
    const pStyle = {
        color: "orange",
        textDecorator: "underline"
    }
    
    return (
    	<div>
        	<h2 style={{fontSize: "50px", color: "red"}}>标题</h2>
            <p style={pStyle}>一段文字</p>
        </div>
    )
}
```



## 方案2：普通CSS

### 概述

书写普通的 `.css` 样式文件，配合 `webpack`，通过`import`导入样式

### 优点与缺点

| 优点                                      | 缺点                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| 写法与CSS传统写法相同，符合标准，符合习惯 | 缺乏模块化，没有局部作用域，只有全局作用域，容易造成样式之间会相互层叠掉 |
|                                           | 不能直接使用 sass 和 less 之类的CSS预处理器                  |

### 示例代码

```css
/* main.css */
p {
    color: orange;
    text-decorator: underline;
}
```

```jsx
import "./main.css";

render() {
    const pStyle = {
        color: "orange",
        textDecorator: "underline"
    }
    
    return (
    	<div>
        	<h2 style={{fontSize: "50px", color: "red"}}>标题</h2>
            <p>一段文字</p> {/* p标签会使用 main.css 中的样式 */}
        </div>
    )
}
```

### 总结

一种常见方案是 `内联样式` + `普通 CSS 配合使用`



## 方案3：CSS Module

### 概述

`CSS Module` 是将CSS文件作为模块导入，而不是直接导入

`CSS Module` 需要配合 `webpack` 使用，`create-react-app`默认集成了`CSS Module`功能

### 优点与缺点

| 优点                             | 缺点                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 拥有局部作用                     | 只能使用类选择器，无法使用伪类，伪元素之类的CSS选择器        |
| 可以使用如 less，scss 等预处理器 | 所有的className都必须使用 `className={style.className}` 的形式来编写 |
|                                  | 不方便动态来修改某些样式，依然需要使用内联样式的方式         |

### 示例代码

```css
/* main.module.css */
.title {
    color: yellow;
}

.setting {
    font-size: 20px;
}

.setting-item-1 {
    background-color: orange;
}

.setting-item-2 {
    background-color: orange;
}

.setting-item-3 {
    background-color: orange;
}
```

```jsx
import React, { PureComponent } from "react";

import myStyle from "./main.module.css";

export default class Profile extends PureComponent {
    render() {
        return (
        	<>
            	<h2 className={myStyle.title}>标题</h2>
                <ul className={myStyle.setting}>
                	<li className={myStyle["setting-item-1"]}>设置列表1</li>
                    <li className={myStyle["setting-item-2"]}>设置列表2</li>
                    <li className={myStyle["setting-item-3"]}>设置列表3</li>
                </ul>
            </>
        );
    }
}
```



## 方案4：CSS in JS

### 概述

`CSS-in-JS` 是指一种模式，它是一种将CSS代码也写入到JavaScript中的方式，可以方便的使用JavaScript的语言特性

所以React有被人称之为 `All in JS`

### 优点与缺点

| 优点                                                         | 缺点                |
| ------------------------------------------------------------ | ------------------- |
| 能够使用 JS 来实现 CSS，语言特性大幅度加强                   | 代码可读性较差      |
| 能够借助 JS 实现包括类似于CSS预处理器一样的样式嵌套、函数定义、逻辑复用、动态修改状态等等 | 不符合标准的CSS写法 |

[一篇批评CSS in JS的文章：Stop using CSS in JavaScript for web development](https://hackernoon.com/stop-using-CSS-in-javascript-for-web-development-fa32fb873dcc)

### 常用框架

`CSS in JS`有很多实现框架，比较知名且常用的有：`styled-component`、`emotion`和`glamorous`

### 原理

`CSS in JS`的原理是：使用 ES6 新增的**标签模板字符串**语法特性

标签模板字符串本质是一个函数，但它的调用方法和参数比较特别

```js
// 本质是函数
function tagFn(...args) {
    console.log(args);
}

// 调用方式比较特别
tagFn`my name is ${namme}, age is ${age}`;

// 参数比较特别
// 第一个参数：普通字符串构成的数组
// 其他的参数：插值变量
const name = "mneumi";
const age = 18;
tagFn`my name is ${namme}, age is ${age}`;
/*
  	["my name is ", ", age is ", ""],
  	"mneumi",
  	18
*/
```



## styled-components

### 概述

`styled-components`是最知名且最流行的`CSS in JS`实现库

### 安装依赖和插件

```shell
npm install styled-components
# OR
yarn add styled-components
```

VS Code辅助插件：`vscode-styled-components`

### 原理

`styled-component`的函数返回值是一个组件，该组件上会被自动添加上一个不重复的class，该class拥有样式

### 特点

支持直接子代选择器或后代选择器，并且直接编写样式

可以通过&符号获取当前元素

可以使用伪类选择器、伪元素等选择器

### 使用技巧1：使用标准标签

| 项   | 说明                                         |
| ---- | -------------------------------------------- |
| 写法 | styled.[标签名]                              |
| 要求 | 必须是`HTML`标准的标签名，不能是自定义标签名 |

```jsx
import React, { PureComponent, createRef } from "react";

import styled from "styled-components";

const HomeWrapper = styled.div` // 返回值是组件
	font-size: 50px;
	color: red;

	.banner { // 类似 scss 中的嵌套写法
		background-color: blue;

        span {
			color: #fff;

            &.active { // 使用 & 获取当前元素
				color: red;
            }

            &:hover { // 使用伪类选择器
                color: green;
            }

			&::after { // 使用伪元素选择器
				content: "aaa";
			}
        }
	}
`;

const TitleWrapper = styled.h2`
	text-decoration: underline;
`;

export default class App extends PureComponent {
    constructor(props) {
        super(props);
    }
    
    render() {
        return (
        	<HomeWrapper>
                <TitleWrapper>App组件</TitleWrapper>
                <div className="banner">
                	<span>轮播图1</span>
                    <span className="active">轮播图2</span>
                    <span>轮播图3</span>
                </div>
            </HomeWrapper>
        );
    }
}
```

### 使用技巧2：继承组件样式

除了可以新建`HTML`标准标签外，还可以基于原有的组件进行样式扩展（常用于配合第三方组件库）

写法：`styled(已存在组件)`

```jsx
const MyButton = styled.button`
	padding: 8px 30px;
	border-radius: 5px;
`;

const MyWarnButton = styled(MyButton)`
	background-color: red;
	color: #fff;
`
```

### 使用技巧3：定义参数（解决动态样式问题）

可以通过`attrs`定义设置参数，`attrs`中定义的参数都会绑定到 props 中

**（该 props 并不是指组件的 this.props，而是传递给样式组件的参数提供对象）**

```jsx
const MyInput = styled.input.attrs({
    placeholder: "请填写密码",
    paddingLeft: props => props.left || "5px"; // 这行的 props 指的是组件的 props
})`
	border-color: red;
	padding-left: ${props => props.paddingLeft}; // 这行的 props 指的是styled-component的参数提供对象

	&:focus {
		outline-color: orange;
	}
`;
```

除了使用`attrs`的形式来添加属性外，还可以直接写参数，具有穿透性，会自动合并到 props 中

```jsx
<MyInput type="password" left="20px" /> // 可以通过 props.type 和 props.left 来访问
```

### 使用技巧4：使用主题

类似于`Context`，需要使用库中的 `ThemeProvider` 组件

```jsx
import { ThemeProvider } from "styled-components";

<ThemeProvider theme={{color: "red", fontSize: "30px"}}>
	<Home />
    <Profile />
</ThemeProvider>

const ProfileWrapper = styled.div`
	color: ${props => props.theme.color};
	font-size: ${props => props.theme.fontSize};
`
```



## 动态添加class

### Vue框架

方式1：传入一个对象

```html
<div class="static"
	:class="{ active: isActive, 'text-danger': hasError }"     
>
</div>
```

方式2：传入一个数组

```html
<div :class="[activeClass, errorClass]"></div>
```

方式3：数组与对象混合使用

```html
<div :class="[{ active: isActive }, errorClass]"></div>
```

### React框架

方式1：拼接模式，通过JSX，可以像编写JavaScript代码一样，通过一些逻辑来决定是否添加某些class

```jsx
<div>
	<h2 className={"title " + (isActive ? "active" : "")}>标题1</h2>
    <h2 className={["title", (isActive ? "active" : "")].join(" ")}>标题2</h2>
</div>
```

方式2：使用`classnames`库

```js
// 'foo bar'
classNames("foo", "bar");

// 'foo bar'
classNames("foo", { bar: true });

// 'foo-bar'
classNames({ "foo-bar": true });

// ''
classNames({ "foo-bar": false });

// 'foo bar'
classNames({ foo: true }, { bar: true });

// 'foo bar'
classNames({ foo: true, bar: true});

// 'foo bar baz quux'
classNames("foo", { bar: true, duck: false}, "baz", { quux: true });

// 'bar 1'
classNames(null, false, "bar", "undefined", 0, 1, { baz: null}, "");
```

```jsx
import classNames from "classnames";
<h2 className={classNames("foo", "bar")}></h2> // foo bar
<h2 className={classNames("foo", { bar: true })}></h2> // foo bar
<h2 className={classNames(null, false, bar, undefined, 0, 1)></h2> // bar 1
```
