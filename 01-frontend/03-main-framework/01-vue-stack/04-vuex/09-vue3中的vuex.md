## 安装依赖

```shell
yarn add vuex
```



## 创建store

```js
import { createStore } from "vuex";

export default createStore({
    state: {
        name: "mneumi"
    },
    getters: {},
    mutations: {
        change(state, payload) {
            this.state.name = "new name"
        }
    },
    actions: {
        change(store, payload) {
            this.commit("change")
        }
    },
    modules: {}
})
```

```js
handleClick() {
    this.$store.dispatch("change")
}
```



## 注册vuex插件

```js
createApp(App).use(store).use(router).mount("#app")
```



## composition API中的Vuex

### 概述

使用 `useStore` 获取 `store` 对象

### 示例代码

```js
import { useStore } from "vuex";

setup() {
    const store = useStore();
    
    const name = store.state.name;
    
    store.commit("change", payload)
    store.dispatch("change", payload)
}
```
