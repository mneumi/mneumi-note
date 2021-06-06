## 初识react-redux

### 概述

react-redux就是`connect`函数的实现

react-redux的功能是让react组件能够更高效简洁使用redux进行状态管理

react-redux将组件与redux进行绑定，使组件在使用redux时，可以不使用action, dispatch, reducer, store等接口，而是使用props对象

### 安装

```shell
npm install react-redux
# OR
yarn add react-redux
```



## react-redux API

### Provider组件

向根组件注入store，使根组件及其内部组件能够`connect`到`store`上

注意：命名不是叫`value`，而是叫`store`

```jsx
import { createStore } from "redux";
import { Provider } from "react-redux";

const store = createStore(rootReducer);

<Provider store = {store}>
  <App />
</Provider>
```

### mapStateToProps

| 项     | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 定义   | 它是一个函数，用于描述store中state与组件props对象之间的映射关系，作为connect函数的参数之一 |
| 参数   | (全局state，组件的props属性)                                 |
| 返回值 | 一个对象，该对象的属性会绑定到组件的props属性上              |

示例代码

```js
const mapStateToProps = (state, props) => ({
	text: state.text
});
```

#### 进阶用法：使用selector函数

| 项   | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 定义 | 组件在使用全局state时，不应该直接使用，而是要通过一个函数来获取 |
| 作用 | 实现组件(视图层)与state(状态管理层)的**解耦**，未来修改state的结构时，不会影响到组件原有的实现 |

示例代码

```js
/* 未使用 selector 函数 */
const mapStateToProps = state => ({
  text: state.text
});

/* 使用 selector 函数 */
export const getText = (state) => state.text;

const mapStateToProps = state => ({
  text: getText(state)
});
```

### mapDispatchToProps

| 项     | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 定义   | 它是一个函数，用于描述dispatch与组件props对象之间的映射关系，作为connect函数的参数之一 |
| 参数   | (dispatch函数，组件的props属性)                              |
| 返回值 | 一个对象，该对象的属性会绑定到组件的props属性上              |

示例代码

```js
const mapDispatchToProps = (dispatch) => ({
  setTodoText: text => dispatch(setTodoText(text)),
  addTodo: text => dispatch(addTodo(text))
});
```

### connect函数

| 项     | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 定义   | 它是一个函数，参数为映射关系函数，返回一个高阶函数，该高阶函数接收组件，将组件与redux通过映射关系函数建立连接后返回增强版组件 |
| 参数   | (mapStateToProps, mapDispatchToProps)                        |
| 返回值 | 用于增强组件的高阶函数，该高阶函数接收组件，将组件与redux通过映射关系函数建立连接后返回高阶组件 |

示例代码

```js
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(AddTodo);
```

#### connect性能问题

前置知识：一般情况下，父组件重新render，将会导致其所有子组件都重新render，这是react的特点

现象：Redux中，state中某个属性发生改变，Redux会让使用到这个属性的组件重新render

结论：应该在较低的层级的组件中进行 react-redux的`connect`，如果在较高层次进行`connect`，将导致很多不必要的重新弄render



## Redcuer拆分与合并

### 目的

便于扩展与维护

### 合并API

使用Redux库中的`combineReducers`函数

### 拆分逻辑

以state为拆分依据，state中的一个属性为一个reducer

### 示例代码

假设Redux中初始state如下

```js
const initalState = {
    filter: "all",
    text: "",
    todos: []
}
```

#### todos.js

> state就是指todos

```js
import { ADD_TODO, TOGGLE_TOOD } from "../actions/actionTypes";

const todos = (state = [], action) => {
    switch (action.type) {
        case ADD_TODO:
            return [
                ...state,
                {
                    text: action.text,
                    completed: false
                }
            ];
        case TOGGLE_TODO:
            return state.map(todo =>
				todo.id === action.id ? 
					{...todo, completed: !todo.completed} : 
					todo;
            }
        default:
            return state;
    }
}
```

####  filter.js

> state就是指filter

```js
import { SET_FILTER } from "../actions/actionsTypes";

const filter = (state = "all", action) => {
    switch(action.type) {
        case SET_FILTER:
            return action.filter;
        default:
            return state;
    }
}
```

#### text.js

> state就是指text

```js
import { SET_TODO_TEXT } from "../actions/actionsTypes";

const filter = (state = "", action) => {
    switch(action.type) {
        case SET_FILTER:
            return action.text;
        default:
            return state;
    }
}
```

#### 合并 combineReducers

```js
import { combineReducers } from "redux";

import todos from "./todos";
import filter from "./filter";
import text from "./text";

export default combineReducers({
    todos,
    text,
    filter
});
```



## Redux目录组织形式

### 按照类型

```js
actions/
    action1.js
	action2.js
reducers/
    reducer1.js
	reducer2.js
components/
    component1.js
	component2.js
containers/
    container1.js
	container2.js
```

需要频繁切换文件，开发起来很不方便

### 按照功能模块

```js
feature1/
    components/
    Container.js
	actions.js
	reducers.js

feature2/
    components/
    Container.js
	actions.js
	reducers.js
```

问题：可能存在 feature1 与 feature2 之间状态耦合的问题

### Ducks（按照状态进行划分）

https://github.com/erikras/ducks-modular-redux

reducers, action types, actions 组合到一个文件中，作为独立模块

划分的依据：应用状态的State，**而不是界面功能**

```js
|- src/
    |- components/
    |- containers/
    |- redux
		|- index.js
		|- module1.js
		|- module2.js
```

module1.js

```js
// Actions
const LOAD = "widget/LOAD";
const CREATE = "widget/CREATE";
const UPDATE = "widget/UPDATE";
const REMOVE = "widget/REMOVE";

const initialState = {
    widget: null,
    isLoading: false,
}

// Action Creators
export function loadWidget() {
    return {type:LOAD};
}

export function createWidget() {
    return {type:CREATE, widget};
}

export function updateWidget() {
    return {type:UPDATE, widget};
}

// Reducer
export default function reducer(state = initialState, action = {}) {
    switch(action.type) {
            
    }
}
```
