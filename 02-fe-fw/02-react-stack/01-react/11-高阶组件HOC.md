## 高阶函数

高阶组件的思维非常类似于高阶函数，在学习**高阶组件**之前，最好先学习**高阶函数**是什么

高阶函数特点如下

* 输入：一个或多个函数
* 输出：一个函数

JavaScript中比较常见的filter、map、reduce都是高阶函数



## 高阶组件

### 概念

高阶组件的英文是 **Higher-Order Components**，简称为 HOC

高阶组件是一个函数，强调：高阶组件是函数，而不是组件

高阶组件是参数为**组件**，返回值为**新组件**的函数

HOC的核心理念就是**组合**

### 命名规范

高阶组件 （HOC）约定以`with`开头，如`withRoute`、`withTranslate`等

### 体会

高阶组件的定义过程

```jsx
function withHOC(Component) { // 高阶组件是一个函数，参数为一个组件
    return class extends React.Component { // 高阶函数返回值为一个新组件
        render() {
            return <Component />
        }
    }
}
```

高阶组件使用过程

```jsx
// 定义一个组件 Container
function Container(props) {
    return <span>This is a container</span>
}

// 调用高阶函数，获取一个新组件 EnhancedCompnent
const EnhancedComponent = withHOC(Container);

export default EnhancedComponent; // 导出新组件
```

### 组件名称

在ES6中，类表达式中类名是可以省略的

```jsx
const Person = class { // class后名称可以省略
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    sayHello() {
        console.log(this.name);
        console.log(this.age);
    }
}

let p = new Person("React", 18);
p.sayHello();
```

组件的名称都可以通过displayName来修改

```jsx
function hoc(Component) { // 高阶组件是一个函数，参数为一个组件
    return class extends React.Component { // 高阶函数返回值为一个新组件
        render() {
            return <Component />
        }

        static displayName = "SuperCpn"; // 使用静态属性定义新组件名称
    }
}
```

获取组件的名称：通过组件的`name`属性

```jsx
WrapperCpn.name
```

### 属性展开

在定义组件的属性值时，可以使用展开语法来批量传递属性值

这称为**属性展开**（Spread Atribute）

```jsx
<App 
  id="id"
  title="App",
  value={123},
  show={true}
/>
```

等价于下列的属性展开语法

```jsx
const data = {
    id: "id",
    title: "App",
    value: 123,
    show=true
}

<App {...data} />
```

可以使用多个展开对象

```jsx
const data1 = {
    id: "id",
    title: "App",
}

const data2 = {
    value: 123,
    show=true
}

<App {...data1} {...data2} />
```

高阶组件可以使用属性展开来传递props

优点：能够批量进行传入，且无需约定好属性名

```jsx
// 定义一个高阶函数
function withHOC(Component) {
    return class extends React.Component {
        constructor(props) {
			super(props);
        }
        render() {
            return <Component {...props} /> // 使用展开属性传递props
        }
    }
}

// 定义一个组件 Person
function Person(props) {
    return (
    	<div>{props.name}</div>
        <div>{props.age}</div>
    );
}

// 调用高阶函数，获取一个新组件 EnhancedPerson
const EnhancedPerson = withHOC(Person);

// 使用 EnhancedPerson 组件
<EnhancedPerson name="person" age={18} />

// name和age两个属性传递到 EnhancedPerson 的 props 中
// EnhancedPerson 通过属性展开 {...this.props} 将 name 和 age 传递到 Person 组件中
```

### 意义

高阶组件能够提供下列的帮助

* 抽取重复代码，能够复用组件的逻辑（**组件**指高阶组件返回的组件）
* 渲染劫持，能够增强组件的功能（**组件**指传入给高阶组件的组件）
* 捕获/劫持被处理组件的生命周期

### 优点

- 支持ES6，光这一项就优于mixins了
- 复用性强，HOC是纯函数且返回值仍为组件，在使用时可以多层嵌套，在不同情境下使用特定的HOC组合也方便调试。
- 同样由于HOC是纯函数，支持传入多个参数，增强了其适用范围。

### 缺点

* 当有多个HOC一同使用时，无法直接判断子组件的props是哪个HOC负责传递的
* 重复命名的问题：若父子组件有同样名称的props，或使用的多个HOC中存在相同名称的props，则存在覆盖问题，而且react并不会报错。当然可以通过规范命名空间的方式避免
* 在react开发者工具中观察HOC返回的结构，可以发现HOC产生了许多无用的组件，加深了组件层级，让开发和调试变得困难
* HOC使用了静态构建，而在render中调用构建方法才是react所倡导的动态构建，与此同时，在render中构建可以更好的利用react的生命周期





## 高阶组件示例

### props增强

在不修改组件的原有代码时，向组件额外添加属性

```jsx
function withEnhanceProps(Component, otherProps) {
    return props => <Component {...props} {...otherProps} />
}
```

### 渲染判断鉴权

在开发中，我们可能遇到这样的场景

* 某些页面是必须用户登录成功才能进行进入
* 如果用户没有登录成功，那么直接跳转到登录页面

```jsx
function withLoginAuth(Page) {
    return props => {
        if (isLogin) {
            return <Page />
        } else {
            return <LoginPage />
        }
    }
}
```

### 生命周期劫持

可以利用高阶组件来劫持生命周期，在生命周期中完成特定的逻辑

```jsx
function withLogRenderTime(Component) {
    return class extends PureComponent {
        UNSAFE_componentWillMount() {
            this.begin = Date.now();
        }
        
        componentDidMount() {
            this.end = Date.now();
            const intervel = this.end - this.begin;
            console.log(`${Component.name} 渲染用时：${interval}`)
        }
        
        render() {
            return <Component />
        }
    }
}
```