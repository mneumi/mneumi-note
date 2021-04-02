::: tip

此为 Vue3 补充的 Vuex，只关注新用法

Vuex的主要用法参考**Vuex 模块**

:::



## 添加依赖

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

```js
import { useStore } from "vuex";

setup() {
    const store = useStore();
    
    const name = store.state.name;
    
    store.commit("change", payload)
    store.dispatch("change", payload)
}
```

1
