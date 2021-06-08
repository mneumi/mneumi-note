## actions作用

actions类似于mutations，区别在于

* action 提交的是 mutation，而不是直接变更状态
* action 可以包含任意异步操作
* mutation是commit的，而action是dispatch的
* action可以整合多个mutation



## 定义action

第一个参数是与 store 实例具有相同方法和属性的 context 对象，第二个参数是 payload

> 第一个参数是context，而不是store本身的原因是由于**module**的存在，详细见 modules 一文

### 普通定义

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state, payload) {
      state.count += payload.count
    }
  },
  actions: {
    increment (context, payload) {
      context.commit('increment', { count: 5 })
    }
  }
})
```

### 解构赋值简化操作

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count += payload.count
    }
  },
  actions: {
    increment ({ commit, state, getters }, payload) {
      commit('increment', payload.count)
    }
  }
})
```



## 派发action

### 概念

action通过 `store.dispatch` 来派发

### 方式1：负载模式

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

```js
store.dispatch('incrementAsync', {
  amount: 10
})
```

### 方式2：对象模式

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

```js
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

### mapActions辅助函数

#### 数组模式

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ])
  }
}
```

#### 对象模式

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```



## 组合action

### 需求

action 通常是异步的，那么如何知道 action 什么时候结束呢？

更重要的是，如何才能组合多个 action，以处理更加复杂的异步流程？

### 原理

 action 函数的返回值可以是 Promise

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

 `store.dispatch` 返回的是 Promise

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

### 使用

#### 基于Promise串联

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

#### 使用Async/Await

```js
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

