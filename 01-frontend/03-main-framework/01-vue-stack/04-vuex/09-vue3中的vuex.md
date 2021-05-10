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

### 使用心得

在 Composition API 中，Vuex常常配合计算属性 computed 使用

```js
const useToken = () => {
    const store = useStore();

    const token = computed(() => {
        return store.state.token;
    });
    
    return { token };
}
```





## 配合TypeScript

### createStore支持泛型

```typescript
interface IUser {
  isLogin: boolean;
  name?: string;
  id?: number;
}

export interface IGlobalData {
  columns: IColumn[];
  posts: IPost[];
  user: IUser;
}

const store = createStore<IGlobalData>({
  state: {
    columns: [],
    posts: [],
    user: {
      isLogin: false,
    },
  },
});
```

### useStore支持泛型

```js
const store = useStore<GlobalData>();
```