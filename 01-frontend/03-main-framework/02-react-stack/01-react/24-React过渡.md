## 概述

为了提高用户体验，可以给组件的显示和消失添加过渡动画

可以通过原生CSS的方式来添加过渡动画，也可以使用React社区提供的`react-transition-group`库来实现`入场`和`出场`过渡效果



## 安装

```shell
npm install react-transition-group --save
# OR
yarn add react-transition-group
```



## react-transition-group主要组件

`react-transition-group`主要包含四个组件

| 主要组件         | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| Transition       | 该组件是一个和平台无关的组件（可以跨平台，不一定要结合CSS）  |
| CSSTransition    | 在前端开发中，一般是结合CSS来完成样式，所以通常使用CSSTransition来完成过渡动画效果 |
| SwitchTransition | 该组件用于两个组件显示和隐藏的切换                           |
| TransitionGroup  | 将多个动画组件包裹在其中，一般用于列表中元素的动画           |



## CSSTransition

### 概述

CSSTransition是基于Transition组件构建的

### 三大场景及状态

CSSTransition执行过程中，有三个场景：**appear（出现）**、**enter（入场）**、**exit（出场）** 

| 场景           | 对应的class名                         | 与Vue类比                        |
| -------------- | ------------------------------------- | -------------------------------- |
| appear（出现） | -appear，-appear-active，-appear-done | Vue中无该场景                    |
| enter（入场）  | -enter，-enter-active，-enter-done    | -enter，-enter-active，-enter-to |
| exit（出场）   | -exit，-exit-active，-exit-done       | -leave，-leave-active，-leave-to |

### 核心属性 in

核心属性in: 设置是`入场过渡`还是`出场过渡`

| in属性的值 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| true       | 入场过渡；添加-enter、-enter-acitve的class，当过渡结束后，会移除两个class，并且添加-enter-done的class |
| false      | 出场过渡；添加-exit、-exit-active的class，当过渡结束后，会移除两个class，并且添加-exit-done的class |

### 其他常见属性

| 属性          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| classNames    | 动画class的名称                                              |
| timeout       | 过渡效果的时间                                               |
| appear        | 是否在初次进入添加过渡效果（需要和属性in同时为true）         |
| unmountOnExit | 过渡效果结束后是否卸载组件，如果为true，则组件会在执行退出动画结束后被移除掉 |
| mountOnEnter  | 显示时加载组件                                               |
| enter         | 加不加enter，默认为true                                      |
| exit          | 加不加exit，默认为true                                       |

### 钩子函数

| 钩子函数   | 说明                   |
| ---------- | ---------------------- |
| onEnter    | 在入场过渡进行前被触发 |
| onEntering | 在入场过渡进行时被触发 |
| onEntered  | 在入场过渡结束后被触发 |
| onExit     | 在入场过渡进行前被触发 |
| onExiting  | 在入场过渡进行时被触发 |
| onExited   | 在入场过渡结束后被触发 |

### transtion时间和timeout的区别

实际过渡执行的过程时间是是由 transition时间决定的，但class的绑定是由timeout决定的

即如果 timeout大于transition时间，则过渡执行完毕时，class中还是active状态，所以最好让 transition 时间和 timeout 相同

### 示例代码

```jsx
import React, { PureComponent } from "react";
import { CSSTransition } from "react-transition-group";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
    }
    
    render() {
        return (
        	<div>
                <CSSTransitionDemo />
            </div>
        );
    }
}

class CSSTransitionDemo extends PureComponent {
    constructor(props) {
        super(props);
        
        this.state = {
            isShow: true
        }
    }
    
    render() {
        const { isShow } = this.state;
        
        return (
        	<div>
                <CSSTransition 
                	in={isShow}
                    classNames="card"
                >
                	<h2>This is demo</h2>
                </CSSTransition>
                <button onClick={e => {this.setState({isShow: !isShow})}}>显示/隐藏</button>
            </div>
        );
    }
}
```

```css
.card-enter {
    opacity: 0;
}

.card-enter-active {
    opacity: 1;
}

.card-enter-done {
    opacity: 1;
}

.card-exit {
    opacity: 1;
}

.card-exit-active {
    transition: opacity 300ms;
}

.card-exit-done {
    opacity: 0;
}
```



## SwitchTransition

### 概述

SwitchTransition组件可以完成两个组件之间切换的过渡效果

### 属性

SwitchTransition组件中有一个重要属性：mode

| mode属性值 | 说明                           |
| ---------- | ------------------------------ |
| in-out     | 表示新组件先进入，旧组件再移除 |
| out-in     | 表示就组件先移除，新组件再进入 |

### 使用方式

SwitchTransition组件里面要有CSSTransition或者Transition组件，不能直接包裹想要切换的组件

SwitchTransition里面的CSSTransition或Transition组件不再像以前接受in属性来判断元素是何种状态，取而代之的是**key**属性

### 示例代码

```jsx
return (
	<div>
    	<SwitchTransition mode="in-out" >
        	<CSSTransiton 
                key={isOn ? "on" : "off"}
                classNames="btn"
                timeout={300}
            >
            	<button>{isOn ? "on" : "off"}</button>
            </CSSTransiton>
        </SwitchTransition>
    </div>
);
```

```css
.btn-enter {
    opacity: 0;
}

.btn-enter-active {
    opacity: 1;
    transition: opacity 300ms;
}

.btn-exit {
    opacity: 1;
}

.btn-exit-active {
    opacity: 0;
    transition: opacity 300ms;
}
```



## TransitionGroup

### 概述

`TransitionGroup`组件用于实现**列表过渡效果**

### 使用

将列表`CSSTransition`组件放入到一个`TransitionGroup`中来实现过渡效果

注意：此时的`CSSTransition`组件不需要 `in` 属性，而需要有 `key` 属性

### 示例代码

```jsx
<div>
	<TransitionGroup>
    {
    	this.state.friends.map((item, index) => {
            return (
            	<CSSTransition 
                    classNames="friend" 
                    timeout={300} 
                    key={index}
                >
                	<div>{item}</div>
                </CSSTransition>
            );
        });
    }
    </TransitionGroup>
    <button onClick={e => this.addFriend()}>+friend</button>
</div>
```