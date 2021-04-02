## ref概述

在 React 开发模式中，通常不需要、也不建议直接操作DOM原生对象，但是在某些特殊的情况下，确实需要获取到DOM进行某些操作

* 管理焦点、文本选择或媒体播放
* 触发强制动画
* 集成第三方 DOM 库

这时，有两种方式去获取DOM

* 使用原生方法，如 `document.querySelector`

* 设置特殊的属性`ref`去获取DOM节点了



## ref类型

ref的类型根据ref指向的节点的类型不同而不同

| 节点类型   | ref属性的值                                         |
| ---------- | --------------------------------------------------- |
| HTML元素   | ref指向的是DOM元素                                  |
| class组件  | ref指向的是挂载的实例对象                           |
| 函数式组件 | 不能对函数式组件使用ref，因为函数式组件没有实例对象 |

对于函数式组件，虽然无法直接使用ref，但可以使用高阶组件`forwardRef`和`useRef`这个hooks



## ref使用

### 概述

ref的使用有三种方式

| ref使用方式  | 说明                                                         | 使用                                        |
| ------------ | ------------------------------------------------------------ | ------------------------------------------- |
| String Ref   | 传入字符串，该字符串会作为ref的名称**（该方法会被废弃，不建议使用）** | 通过 `this.refs.字符串` 来获取ref指向的对象 |
| Create Ref   | 传入对象，该对象是通过 `React.createRef` 创建出来的          | 通过`ref对象.current` 来获取ref指向的对象   |
| Callback Ref | 传入函数，该函数会在DOM被挂载时进行回调，该函数传入一个元素对象，需要自己保存它 | 通过保存的对象来获取ref指向的对象           |
| Ref Hook     | 使用 useRef 这个Hook                                         | 具体看 **ReactHooks** 一文                  |

### 示例代码

#### String Ref

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
    render() {
        return (
        	<div>
            	<h2 ref="titleRef">Hello Refs</h2>
                <button onClick={()=>this.changeText()}>点击改变文本</button>
            </div>
        );
    }
    
    changeText() {
        console.log(this.refs.titleRef) // dom元素
        titleRef.innerHTML = "Hello React"
    }
}
```

#### Create Ref

```jsx
import React, { PureComponent, createRef } from "react";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
        
        this.titleRef = createRef();
    }
    
    render() {
        return (
        	<div>
                {/* 推荐方式*/}
            	<h2 ref={this.titleRef}>Hello Refs</h2>
                <button onClick={()=>this.changeText()}>点击改变文本</button>
            </div>
        );
    }
    
    changeText() {
        console.log(this.titleRef) // Ref对象
        console.log(this.titleRef.current); // dom元素
        this.titleRef.current.innerHTML = "Hello JavaScript"
    }
}
```

#### Callback Ref

```jsx
import React, { PureComponent, createRef } from "react";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
        
        this.titleEl = null;
    }
    
    render() {
        return (
        	<div>
            	<h2 ref={(dom)=>this.titleEl=dom}>Hello Refs</h2>
                <button onClick={()=>this.changeText()}>点击改变文本</button>
            </div>
        );
    }
    
    changeText() {
        console.log(this.titleEl) // dom元素
        this.titleEl.innerHTML = "Hello CSS"
    }
}
```

### useRef

```jsx
import React, { useRef } from 'react';

export default function RefHookDemo1() {
    const titleRef = useRef();
    const inputRef = useRef();
    
    function changeDOM() {
        titleRef.current.innnerHTML = 'Hello World';
    }
    
    return (
    	<div>
        	<h2 ref={titleRef}>RefHookDemo1</h2>
            <input ref={inputRef} type="text"></input>
            
            <button onClick={e => changeDOM()}>修改DOM</button>
        </div>
    );
};
```



## ref转发

### 概述

默认情况下，ref不能作用于函数式组件，因为函数式组件没有实例对象

如果确实需要使用 ref 指向函数式组件，需要配合高阶函数 `forwardRef` 进行，这称为**ref转发**

### 使用

使用`forwardRef`来包裹函数式组件，其中函数式组件的第二个参数即为`ref`对象，需要将其赋值给需要的节点对象

### 示例代码

```jsx
import React, { forwardRef } from "react";

const Home = forwardRef(function(props, refObj) {
    return (
    	<div>
        	<h2 ref={refObj}>Home</h2>
            <button>按钮</button>
        </div>
    );
})
```