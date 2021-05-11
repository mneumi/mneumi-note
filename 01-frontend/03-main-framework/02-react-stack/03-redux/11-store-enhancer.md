## store enhancer

### 概述

store enhancer用于增强`createStore`的功能

### 一般结构

```js
function enhancerCreator() {
    return createStore => (...args) => {
        // do someting based old store
        // return a new enhanced store
    }
}
```



## 示例

### 定义

```js
const logger = createStore => (...args) => {
    const store = createStore(...args);
    const dispatch = (action) => {
        console.group(action.type);
        console.log("dispatching: ", action);
        const result = store.dispatch(action);
        console.log("next state:", store.getState());
        console.groupEnd();
        return result;
    }
    return {...store,dispatch};
}

export default logger;
```

### 使用

```js
import { createStore, applyMiddleware, compose } from "redux";
import loggerEnhancer from "./enhancers/logger";

const store = creteStore(rootReducer, compose(applyMiddleware(thunkMiddleware),loggerEnhancer));
```



## store enhancer与middleware

### 区别

中间件是一种特殊的store enhancer，middleware是store enhancer 的一种特例

```js
function applyMiddleware(...middlewares) {
	return createStore => (...args) => {
        // 省略
        return {
            ...store,
            dispatch
        }
    }
}
```

### 实践

一般，推荐`middleware`，而不是`store enhancer`

因为middleware更加高层，只能增强dispatch能力，而store enhancer除了dispatch外，还能够增强别的能力，比如说 getState，subscribe等等

`store enhancer`看似很灵活，但容易破坏redux原有的逻辑