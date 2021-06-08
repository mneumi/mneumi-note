## mutations作用

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation



## mutations必须是同步函数

一条重要的原则就是要记住 **mutation 必须是同步函数**

原因是 devtools 需要记录状态，如果 mutations 是异步，则 devtools 难以记录状态



## 定义mutations

每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**

回调函数的第一个参数是`state`，第二个参数是`payload`

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state, payload) {
      // 变更状态
      state.count = state.count + payload.count
    }
  }
})
```



## 提交mutations

使用 `store.commit` 来提交mutations

### 方式1：负载模式

两个参数

* 第一个参数：type
* 第二个参数：payload

示例

```js
store.commit('increment', {
  count: 10
})
```

对应的处理函数

```js
mutations: {
  increment (state, payload) {
    state.count += payload.count
  }
}
```

### 方式2：对象模式

只有一个参数，就是一个对象，拥有`type`和`payload`属性

示例

```js
store.commit({
  type: 'increment',
  count: 10
})
```

对应的处理函数

```js
mutations: {
  increment (state, payload) {
    state.count += payload.count
  }
}
```

### mapMutations辅助函数

可以使用 `mapMutations` 简化 commit 写法

### 数组模式

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ])
  }
}
```

### 对象模式

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

