## Context概述

### 非父子组件间数据共享

在React开发中，比较常见的数据传递方式是通过props属性自上而下（由父到子）进行传递

但是对于有一些场景：比如一些数据需要在多个组件中进行共享（如地区偏好、UI主题、用户登录状态、用户信息等）

如果我们在顶层的App中定义这些信息，之后一层层传递下去，那么对于一些中间层不需要数据的组件来说，是一种冗余的操作

### Context作用

React在 `v16` 版本后提供了一个API：Context

Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言等

### Context缺点

如果使用多个Context，则容易发生嵌套过深的问题

不应该大量使用 Context，使用一个Context就好



## Context对象

### 创建

通过`React.createContext`创建一个Context对象，表示上下文

```js
const MyContext = React.createContext();
```

### 默认值

创建Context时，可以设置默认值 defaultValue

* defaultValue**仅仅**在Consumer在树中没有匹配的 Provider时使用（**在找不到对应Provider时使用**）
* 将 `undefined` 作为 Provider 的value值传递并不会导致 Consumer 使用 `defaultValue`

```js
const MyContext = React.createContext(defaultValue);
```

### Provider和Consumer

每个 Context 对象有对应的 `Provider 组件` 和 `Consumer` 组件

```jsx
const MyContext = React.createContext(defaultValue);

const { Provider, Consumer } = React.createContext(defaultValue);
```



## Provider组件

### 概念

每个 Context 对象都有对应的 Provider 组件，用于提供数据

Provider组件有一个 value 属性，用于定义Provider中存储的值

当 Provider 的 value 值发生变化时，Provider内部的所有消费组件都会重新渲染

```jsx
const defaultValue = {};

const initValue = {
    name: "Context Provider"
}

const MyContext = React.createContext(defaultValue);

<MyContext.Provider value={initValue}>
</MyContext.Provider>
```

### 使用示例

用Provider组件包裹住需要使用Context中的值的组件即可

```jsx
const defaultValue = {};

const initValue = {
    name: "Context Provider"
}

const MyContext = React.createContext(defaultValue);

<MyContext.Provider value={initValue}>
    <Header />
    <Main />
    <Footer />
</MyContext.Provider>
```

如果存在多个Provider，则Provider之间的嵌套顺序不重要，只要都包裹住需要使用Provider中的数据的组件即可

```jsx
// 定义多个Context
const ThemeContext = React.createContext({});
const ProfileContext = React.createContext({});

// 获取对应的Provier组件
const ThemeProvider = ThemeContext.Provider;
const ProfileProvidr = ProfileContext.Provider;

// 使用Provider组件包裹住需要使用Provider中的数据的组件
<ThemeProvider>
	<ProfileProvider>
    	<Header />
        <Main />
        <Footer />
    </ProfileProvider>
</ThemeProvider>

// 嵌套的顺序没有要求
<ProfileProvider>
	<ThemeProvider>
    	<Header />
        <Main />
        <Footer />
    </ThemeProvider>
</ProfileProvider>
```



## Consumer组件

### 概念

每个 Context 对象都有对应的 Consumer 组件，用于消费Provider中的数据

Consumer使用`Render Props`的模式，即需要使用`渲染函数`来使用 Consumer 组件

Consumer支持在类组件和函数组件中使用

### 使用示例

```jsx
class Main extends Component {
	render() {
        return (
        	<ThemeContext.Consumer>
            {
            	value => <h1>theme: {value}</h1>     
            }
            </ThemeContext.Consumer>
        );
    }
}
```

如果需要使用多个Context时，需要嵌套使用

```jsx
function Main(props) {
    return (
        <ThemeContext.Consumer>
            {
                themeValue => (
                    <h1>theme: {themeValue}</h1>
                    <PrifleContext.Consumer>
                        {
                            profileValue => <h2>profile: {profileValue}</h2>
                        }
                    </PrifleContext.Consumer>
                );
            }
        </ThemeContext.Consumer>
    );
}
```



## contentType属性

### 概念

通过设置**类组件**的 `contextType` 静态属性为一个Context 对象，可以让该组件在不使用`Consumer`组件的情况下获取到Provider中的数据，数据会自动绑定到类组件的`this.context`中

即：`this.context`指向的就是`Provider`中`value`的值

```jsx
// 创建Context
const MyContext = React.createContext();

class App extends React.Component {
    render() {
		return <h1>{this.context}</h1>
    }
    
    static contextType = MyContext; // 设置contentType，绑定Context
}
```

### 注意

1. contentType**只适用于只有一个Context的情况**，因为 contextType 只能指定一个Context，如果一个组件需要使用多个Provider，则还是需要在组件内部嵌套使用Consumer组件

2. 在构造函数中，还无法使用`this.context`，因为该属性是在构造函数完成后才绑定的，如果想要在构造函数中获取Context中的值，可以使用构造函数的第二个参数，即

   ```js
   constructor(props, context) {
       console.log(context);
   }
   ```

### 使用示例

```jsx
import React from "react";
import ReactDOM from "react-dom";

const ThemeContext = React.createContext();

class Main extends React.Component {
  constructor(props) {
    super(props);
    console.log(this.context);
  }

  render() {
    return <h1>theme: {this.context.theme}</h1>;
  }
}

Main.contextType = ThemeContext; // 设置contentType的另一种形式

function App(props) {
  return (
    <React.Fragment>
      <ThemeContext.Provider value={{ theme: "light" }}>
        <Main />
      </ThemeContext.Provider>
    </React.Fragment>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById("root")
);
```
