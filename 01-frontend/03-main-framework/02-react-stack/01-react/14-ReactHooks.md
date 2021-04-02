## class组件分析

### class组件相对于普通函数式组件的优势

| 优势                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| class组件可以定义自己的state，用来保存组件自己内部的状态     | 函数式组件不可以，因为函数每次调用都会产生新的临时变量       |
| class组件有自己的生命周期，可以在对应的生命周期中完成特定的逻辑 | 比如在componentDidMount中发送网络请求，并且该生命周期函数只会执行一次<br />函数式组件在Hooks之前，如果在函数中发送网络请求，意味着每次重新渲染都会重新发送一次网络请求 |
| class组件可以在状态改变时只会重新执行render函数以及特定的生命周期函数 | 函数式组件在重新渲染时，整个函数都会被执行                   |

### class组件中存在的问题

| 存在问题                   | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| 复杂组件变得难以理解与维护 | 随着业务的增多，class组件可能会变得越来越复杂<br />对于复杂class实际上非常难以拆分，因为它们的逻辑往往混在一起，强行拆分反而会造成过度设计，增加代码的复杂度 |
| class和this问题            | JavaScript中，class有一定的学习成本，以及this比较难以掌握    |
| 组件逻辑难以复用           | Hooks之前，需要通过 Render Props 或者 HOC 实现组件复用，存在一定的缺点 |



## 初识Hooks

### Hooks出现

Hooks 是 React 16.8 的新增特性，它可以在不编写class的情况下使用state以及其他的React特性（如生命周期）

Hook的出现，可以解决class组件的问题

### Hooks使用场景

Hook基本可以代替之前所有使用class组件的地方，除了一些非常不常用的场景：比如无法模拟全部的生命周期

### Hooks使用要求

#### 强制要求

1. 必须在顶层使用Hooks函数，即不要在条件语句，循环语句，嵌套语句中使用Hooks函数

2. Hooks函数只能在函数式组件或自定义Hooks函数（以`use`开头）中使用，不要在其中函数中使用

#### 原因说明

在条件语句中写Hooks函数，很容易引起问题，因为底层是使用链表维护的

在普通函数中写Hooks函数，如果这个普通函数用在了函数式组件中的条件语句中，则非常容易引起问题，且很难被发现

#### 约束检查

`ESLint`是根据`use`前缀进行检查的

* 如果是Hooks函数，则必须用`use`开头
* 如果不是Hooks函数，不要使用`use`开头



## useState

### 概述

`useState`本身是一个函数，来自于 `react` 包

`useState`用于让函数式能够保留状态，相当于有属于自己的`state`

### 使用

参数：状态的默认值（如果不填默认为 undefined）

返回值：一个长度为2的数组

* 元素1：当前state的值
* 元素2：更新state的值的函数

### 示例代码

```jsx
import React, { useState } from 'react';

export default function Counter() {
    const [count, setCount] = useState(0);
    
    return (
    	<div>
        	<h2>当前计数：{ count }</h2>
            <button onClick={e => setCount(count + 1)}>+1</button>
            <button onClick={e => setCount(count - 1)}>-1</button>
        </div>
    );
};
```

### 注意事项

`useState`在更新数据前会进行浅层比较，所以对于复杂数据的操作，需要返回新的数据，不能直接修改本身

```jsx
import React, { useState } from 'react';

export default function ComplexHookState() {
    const [friends, setFriends] = useState(['alice', 'jack']);
    
    return (
    	<div>
        	<h2>好友列表</h2>
            <ul>
            	{
                    frinends.map((item, index) => {
                        return <li key={item}>{item}</li>
                    });
                }
            </ul>
            <button onClick={e => setFriends([...frinends, "tom"])}>添加朋友</button>
        </div>
    );
};
```

### 进阶用法

`useState`中初始化参数可以写函数，更新函数的参数也可以写函数，类似于`setState`的用法

回调函数只有在第一次使用会被调用，之后重新渲染也不会再重新调用

```jsx
import React, { useState } from 'react';

export default function CounterHook() {
	const [count, setCount] = useState(() => 10);
    
    function handleBtnClick() {
        setCount((prevCount) => prevCount + 10);
        setCount((prevCount) => prevCount + 10);
        setCount((prevCount) => prevCount + 10);
        setCount((prevCount) => prevCount + 10);
        setCount((prevCount) => prevCount + 10);
    }
    
    return (
    	<div>
        	<h2>当前计数：{count}</h2>
            <button onClick={e => handleBtnClick()}>+50</button>
        </div>
    );
};
```

### useState原理

useState是利用js是单线程的特性，推断出是拿当前组件的state，而不是其他组件的state

react并不知道具体的state是叫啥名字，它里面存的就是一个数据已经它的改变方法，名字是人赋予它的

useState内部是使用链表保存多个State的，这就要求保证组件中所有的state是固定顺序固定个数，不能多也不能少

* 这就导致不能动态修改组件中useState的个数
* 不能在条件语句中使用useState，因为这会动态修改组件的useState的个数
* 即有一条约定：在函数组件的最上面使用useState，且不要在条件语句中使用useState



## useReducer

### 概述

`useReducer`类似于`useState`，在某些场景下，如果state的处理逻辑比较复杂，我们可以通过useReducer来对其进行拆分

### 使用

参数1：reducer函数

参数2：初始state

返回值：一个长度为2的数组

* 元素1：当前state的值
* 元素2：更新用的dispatch函数

### 特点

数据是不会共享的，它们只是使用了相同的counterReducer的函数而已

useReducer只是useState的一种替代品，并不能替代Redux

### 示例代码

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
    switch (action.type) {
        case "increment":
            return {...state, counter: state.counter + 1};
        case "decrement":
            return {...state, counter: state.counter - 1};
        default:
            return state;
    }
}

export default function Counter() {
    const [state, dispatch] = useReducer(reducer, {count: 0});
    
    return (
    	<div>
        	<h2>当前计数：{ state.count }</h2>
            <button onClick={e => dispatch({type: "increment"})}>+1</button>
            <button onClick={e => setCount({type: "decrement"})}>-1</button>
        </div>
    );
};
```



## useEffect

### 概述

因为函数式组件中无生命周期，而生命周期对于很多场景来说是必须的（比如网络请求）

所以出现了 `useEffect` 这个 Hook，用于实现类似生命周期的效果

### 使用

参数1：普通回调函数

参数2：依赖项数组

### 性能优化

#### 使用场景

默认情况下，useEffect的回调函数会在每次渲染时都重新执行，但是这会导致两个问题

* 某些代码只希望执行一次，类似于componentDidMount和componentWillUnmount中完成的事情，比如网络请求、订阅和取消订阅等
* 另外，多次执行也会导致一定的性能问题

#### 解决问题

通过`useEffect`的第二个参数定义好依赖项，只有当依赖项发生改变时，`useEffect`中的回调函数才重新执行

如果一个函数不希望依赖任何的内容时，也可以传入一个空的数组 []，那么这里的两个回调函数分别对应的就是componentDidMount和componentWillUnmount生命周期函数了

### 清除机制

#### 需求场景

在class组件的编写过程中，某些副作用的代码，需要在componentWillUnmount中进行清除

* 取消订阅
* 取消事件绑定

#### 定义清除函数

`useEffect`传入的`回调函数A`本身可以有一个返回值，这个返回值是另外一个`回调函数B`，`回调函数B`称为`清除回调函数`

`清除回调函数`中可以执行类似componentWillUnmount中的清除操作

#### 清除函数执行时机

`useEffect`中的会回调函数会在`DOM`渲染完毕后执行，其中`清除回调`的在`普通回调`之前执行

默认情况下，无论是第一次渲染之后，还是每次更新之后，都会执行回调函数

### 示例代码

```jsx
import React, { useState, useEffect } from 'react';

export default function ChangeTitle() {
	const [count, setCount] = useState(() => 10);
    
	useEffect(() => {
        document.title = count;
    }, [count]);
    
    return (
    	<div>
        	<h2>当前计数：{count}</h2>
            <button onClick={e => setCount(count + 1)}>+1</button>
            <button onClick={e => setCount(count - 1)}>-1</button>
        </div>
    );
};
```



## useLayoutEffect

### 概述

非常类似于`useEffect`，区别在于回调函数的执行时机

`useEffect`：在DOM更新后执行回调函数

`useLayoutEffect`：在DOM更新之前执行回调函数

### 使用

参数1：回调函数

参数2：依赖项数组

### 示例代码

#### 使用useEffect

```jsx
import React, { useState, useEffect } from 'react';

export default function EffectCounter() {
    const [count, setCount] = useState(10);
    
    useEffect(() => {
        if (count === 0) {
            setCount(Math.random() + 200);
        }
    }, [count]);
    
    return (
    	<div>
        	<h2>数字：{count}</h2>
            <button onClick={e => setCount(0)}>修改数字</button>
        </div>
    );
}
```

#### 使用useLayoutEffect

```jsx
import React, { useState, useLayoutEffect } from 'react';

export default function EffectCounter() {
    const [count, setCount] = useState(10);
    
    useLayoutEffect(() => {
        if (count === 0) {
            setCount(Math.random() + 200);
        }
    }, [count]);
    
    return (
    	<div>
        	<h2>数字：{count}</h2>
            <button onClick={e => setCount(0)}>修改数字</button>
        </div>
    );
}
```



## useCallback

### 概述

`useCallback`的能力是对`函数`进行缓存，如果依赖项没有更新，则始终返回同一个函数，从而实现性能优化

使用场景：主要用于定义传递给子组件的回调函数，通常使用useCallback的目的是不希望子组件进行多次渲染，并不是为了对组件内的函数进行缓存

### 使用

参数1：进行缓存的函数

参数2：依赖项数组

返回值：记忆后的函数

### 示例代码

```jsx
import React, { useState, useCallback } from 'react';

const MyButton = memo((props) => {
   return <button onClick={props.increment}>MyButton +1</button> 
});

export default function CallbackHook() {
    const [count, setCount] = useState(0);
    const [show, setShow] = useState(true);
    
    const increment1 = () => {
        console.log("increment1");
    	setCount(count + 1);  
    };
    
    const increment2 = useCallback(() => {
        console.log("increment2");
        setCount(count + 1);
    }, [count]);
    
    return (
        <div>
        	<h2>CallbackHookDemo</h2>
            <MyButton title="btn1" increment={increment1} />
            <MyButton title="btn2" increment={increment2} />
            <button onClick={e => setShow(!show)}>切换</button>
        </div>
    );
};
```



## useMemo

### 概述

`useMemo`的能力是对`返回值`进行缓存，如果依赖项没有更新，则始终返回同一个返回值，从而实现性能优化

### 使用

参数1：回调函数，函数中返回需要缓存的值

参数2：依赖项数组

返回值：缓存的值

### 示例代码

#### 示例代码1

```jsx
import React, { useState, useMemo } from 'react';

function calcNumber(count) {
    let total = 0;
    for (let i=1; i<=count; i++) {
        total += i;
    }
    return total;
}

export default function MemoHook() {
    const [count, setCount] = useState(10);
    const [show, setShow] = useState(true);
    
    const total = useMemo(() => {
        return calcNumber(count);
    }, [count]);
    
    return (
    	<div>
        	<h2>计算数字的和：{total}</h2>
            <button onClick={e => setShow(!show)}>切换状态，进行刷新</button>
        </div>
    );
};
```

#### 示例代码2

```jsx
import React, { useState, useMemo, memo } from 'react';

const Info = memo((props) => {
    console.log(`Info${props.id} 重新渲染`)
    return <h2>名字：{props.info.name} 年龄：{props.info.age}</h2>
})

export default function MemoHook2() {
    const [show, setShow] = useState(true);
    
    const info1 = {name: 'alice', age: 18};
    const info2 = useMemo(() => {
        return {name: 'tom', age: 20};;
    }, []);
    
    return (
    	<div>
        	<Info id={1} info={info1} />
            <Info id={2} info={info2} />
            <button onClick={e => setShow(!show)}>切换状态，进行刷新</button>
        </div>
    );
};
```

### 对比 useCallback

* useCallback针对函数，返回值也是函数，决定函数是否会重新定义（更新前后是否是同一个函数）
* useMemo针对return的数据   所以useMemo可以实现useCallback，只要返回值是函数即可

* useMemo和useCallback都是性能优化，主要用于子组件优化



## useContext

### 概述

`useContext`的作用是为了方便地获取`Context`中的值，而不用写复杂的嵌套代码

### 使用

参数：Context对象

返回值：Context对象中的值

### 注意事项

Context会破坏组件的独立性，不要滥用Context

### 示例代码

```jsx
import React, { createContext, useContext } from 'react';

const UserContext = createContext();
const ThemeContext = createContext();

function App() {
    return (
    	<UserContext value={{name:'alice', age: 18}}>
        	<ThemeContext value={{fontSize:'30px', color: 'red'}}>
                <ContextHookDemo></ContextHookDemo>
        	</ThemeContext>
        </UserContext>
    );
}

function ContextHookDemo() {
	const user = useContext(UserContext);
    const theme = useContext(ThemeContext);
    
    return (
    	<div>
        	<h2>{user.name}</h2>
            <h2>{theme.color}</h2>
        </div>
    );
};
```



## useRef

### 概述

`useRef`用于实现`ref`的能力

### 使用

参数：初始化值（useState可以传入函数，而useRef不能传入函数，只能传入初始值）

返回值：回一个ref对象

特点：返回的ref对象再组件的整个生命周期保持不变

### 使用场景

1. 引入DOM（或者组件，但是需要是class组件）元素
2. 保存一个数据，这个对象在整个生命周期中可以保存不变

### 示例代码

#### 示例代码1：引入DOM

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

#### 示例代码2：整个生命周期中可以保存不变

```jsx
import React, { useRef, useState } from 'react';

export default function RefHookDemo1() {
    const [count, setCount] = useState(0);
    const numRef = useRef(count);
    
    return (
    	<div>
        	<h2>numRef中的值：{numRef.current}</h2>
            <h2>count中的值：{count}</h2>
            <button onClick={e => setCount(count + 10)}>+10</button>
        </div>
    );
};
```



## useImperativeHandle

### 概述

`useImperativeHandle`用于限制`ref`的能力，避免将子组件的DOM过多暴露给父组件

本质：对父组件传进来的ref对象进行拦截，内部使用自己创建的ref对象

### 使用

参数1：ref对象

参数2：回调函数，函数中返回暴露给父组件的属性

参数3：依赖项数组，用于决定第二个参数回调函数是否重新执行

### 示例代码

```jsx
import React, { useRef, forwardRef } from 'react';

const MyInput = forwardRef((props, outterRef) => {
	const innerRef = useRef();
    
    useImperativeHanle(outterRef, () => ({
        focus: () => {
            innerRef.current.focus();
        }
    }), [innerRef.current]);
    
    return <input ref={innerRef} type="text" />
});

export default function ImperativeHandleDemo() {
    const outterRef = useRef();
    
    return (
    	<div>
        	<MyInput ref={outterRef} />
            <button onClick={e => outterRef.current.focus()}>聚焦</button>
        </div>
    );
}
```



## 自定义Hooks

### 概述

自定义Hook本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算React的特性

自定义Hook函数要求必须以`use`开头

### 示例代码

示例代码1：Context共享

```jsx
function useUserToken() {
    const user = useContext(UserContext);
    const token = useContext(TokenContext);
    
    return [user, token];
}
```

示例代码2：获取鼠标滚动位置

```jsx
function useScrollPosition() {
    const [scrollPosition, setScrollPosition] = useState(0);
    
    useEffect(() => {
        const handleScroll = () => {
            setScrollPosition(window.scrollY);
        }
        
        document.addEventListener('scroll', handleScroll);
        
        return () => {
            document.removeEventListener("scroll", handleScroll);
        }
    }, []);
    
    return scrollPosition;
}
```

示例代码3：localStorage数据存储

```jsx
function useLocalStorage(key) {
    const [data, setData] = useState(() => {
        return JSON.parse(window.localStorage.getItem(key));
    });
    
    useEffect(() => {
        window.localStorage.setItem(key, JSON.stringify(data));
    }, [data]);
    
    return [data, setData];
}
```



## Hooks常见问题

### useState性能问题?

useState是在操作DOM前完成的，不必担心性能问题

### useEffect能否对应所有的生命周期函数？

`getSnapshotBeforeUpdate`和`componentDidCatch`和`getDerivedStateFromProps`无法通过Hooks进行模拟

函数式组件还不能完全替代类组件，在当前可以使用类组件和HOC来实现这类少见需求

### 依赖项对比使用的算法?

依赖项对比用的是**浅层比较**

### Hooks中如何获取历史props和state?

使用`useRef`，因为ref不受渲染周期的影响

```jsx
function Counter() {
    const [count, setCount] = useState(0);
    
    const prevCountRef = useRef();
    
    useEffect(()=>{
        prevCountRef.current = count;
    });
    
    const prevCount = prevCountRef.current;
    
    return <h1>Now: {count}, before: {prevCount}</h1>
}
```

### 如何强制更新一个Hooks组件

class组件：使用forceUpdate

Hooks组件：定义一个无关的state，更新它的值，从而实现强制渲染

```jsx
function Counter() {
    const [count, setCount] = useState(0);
    const [updater, setUpdater] = useState(0);
    
    function forceUpdate() {
        setUpdater(updater => updater+1);
    }
    
    const prevCountRef = useRef();
    
    useEffect(()=>{
        prevCountRef.current = count;
    });
    
    const prevCount = prevCountRef.current;
    
    return <h1>Now: {count}, before: {prevCount}</h1>
}
```



## 组件逻辑复用方式总结

| 组件逻辑复用方式 | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| Mixin            | Mixin 可能会相互依赖，相互耦合，不同Mixin中的方法可能会相互冲突，不利于代码维护 **（强烈不推荐）** |
| HOC              | 模式简单，但会增加组件的层级**（酌情使用）**                 |
| Render Props     | 代码简洁，但阅读性、扩展性和可维护性较差**（酌情使用）**     |
| Hooks            | 非常优秀的解决方案，且官方推荐和社区流行**（强烈推荐使用）** |