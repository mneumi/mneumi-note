## 存在问题：太多重复代码

在 **React中使用Redux**一文中，描述了如何在`Home`和`Profile`组件中使用Redux，简单概括就是要在组件加载时订阅Context，在组件卸载时取消订阅，在这个过程中，存在太多重复代码

为了解决这个问题，需要使用高阶函数`connect`



## 自定义connect高阶函数

> 每个组件想要的state不一样，每个组件想要的dispatch不一样

### 概念

connect的作用是连接React和Redux，目的是为了解决Redux在React使用过程中太多重复代码的问题

connect是高阶函数，其返回一个函数，该函数是一个高阶组件，该函数接收一个组件，返回一个新的组件

即：`connect ==> 高阶组件 ==> 新组件`

### 作用

connect函数能够将redux中的state和dispatch方法注入到组件props中，组件可以直接使用

### 实现

> connect.js

```jsx
import { PureComponent } from "react";
import { StoreContext } from "./context.js"; 

// StoreContex是一个Context，其Provider的value为Redux中的store

export function connect(mapStateToProps, mapDispatchToProps) { // connect高阶函数
    
    return function enhanceHOC(WrappedComponent) { // 返回高阶组件
        
       // 定义高阶组件中的组件
       class EnhanceComponent extends PureComponent {
           
            constructor(props, context) {
                super(props, context);
                
                this.state = {
                    // 此时获取不到 this.context
                    // this.context要在constructor执行完毕后才有，所以需要通过参数来获取
                    storeState: mapStateToProps(context.getState()) 
                }
            }
            
            componentDidMount() {
                this.unsubsribe = this.context.subscribe(() => {
                    this.setState({
                        storeState: mapStateToProps(this.context.getState())
                    });
                });
            }
            
            componentWillUnmount() {
                this.unsubscribe(); // 取消订阅
            }
            
            render() {
                return (
                    <WrapperdComponent 
                        {...this.props} 
                        {...mapStateToProps(this.context.getState())}
                        {...mapDispatchToProps(this.context.dispatch)}
                	/>);
            }
        }
        
        // 相当于向constructor中传入第二个参数 context
        EnhanceComponent.contextType = StoreContext; 
        
        return EnhanceComponent;
    }
}
```

### 性能问题

只要 store 的state改变，就会触发所有的订阅函数，而订阅函数中进行了setState，就会准备更新组件，然后调用 shouldComponentUpdate 这个函数去判断究竟是否执行render，而默认继承 React.Compnent的组件的SCU默认返回true

这意味着：假设有三个组件A B C 进行了connect，然后A的操作（该操作与B、C根本无关）导致了store更新，触发了所有的订阅函数，导致这个B、C也进行了render，即：明明更新的数据与组件无关，而该组件也要重新 render，极大浪费性能

优化方式：

* 类组件不要继承 React.Component，而是要继承 React.PureComponent

* 函数组化要使用 memo 高阶组件进行包装，实现类似 React.PureComponent的浅层比较的效果

### 使用示例

#### context.js

```js
import React from "react";

const StoreContext = React.creteContext();

export {
	StoreContext
}
```

#### 注入Store到Context中

```jsx
import { StoreContext } from "./utils/context.js";

import App from "./App";

import store from "./store";

ReactDOM.render(
	<StoreContext.Provider value={store}>
    	<App />
    </StoreContext.Provider>,
    document.getElementById("app");
);
```

#### About.jsx

```jsx
import { connect } from "../utils/connect.js";

function About(props) {
    return (
        <div>
        	<h1>About</h1>
            <h2>当前计数：{props.counter}</h2>
            <button onClick={e => props.decrement()}>-1</button>
            <button onClick={e => props.subNumber(5)}>-5</button>
        </div>
    );
}

const mapStateToProps = state => ({
	counter: state.counter  
});

const mapDispatchToProps = dispatch => ({
    decrement: () => {
        dispatch(decrementAction())
    },
    subNumber: (num) => {
        dispatch(subAction(num))
    }
});

export default connect(mapStateToProps，mapDispatchToProps)(About);
```
