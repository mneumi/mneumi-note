## 安装Vuex

```shell
npm install vuex --save
# OR
yarn add vuex
```



## 创建Store

```js
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        count: 0
    },
    getters: {},
    mutations: {},
    actions: {},
    modules: {}
});

export default store;
```



## 注册Store

```js
new Vue({
    el: "#app",
    store
})
```





## 更改状态

### 概述

更改store中的state需要通过`commit` `mutations`

### 注意

mutations中不能有异步（副作用）代码，原因是要保证mutations中的操作是同步的，以方便`vue devtools`记录状态

### 示例代码

```js
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment(state, payload) {
            state.count++;
        }
    }
});
```

原始方式

```js
store.commit("increment");

console.log(store.state.count); // 1
```

组件方式

```js
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```



## store模式

#### 创建 store.js

```js
// store.js
const store = {
  state: {
    count: 0
  },
  increment() {
    this.state.count++
  }
}
```

#### 使用store

```js
// App.vue

import store from "@/store.js"

export default {
  data() {
    return {
      storeState: store.state
    }
  },
  methods: {
    increment() {
      store.increment()
    }
  }
}
```



## 注入

```ts
import { createStore } from 'vuex';

const store = createStore({
    state: {
        count: 0
    },
    mutations: {
        add(state) {
        	state.count++;
        }
    }
});
```





### 安装Vuex

```shell
yarn add vuex
```

