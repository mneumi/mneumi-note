## 单一状态树

Vuex 使用**单一状态树**，用一个对象就包含了全部的应用层级状态，它作为一个“唯一数据源"，每个应用将仅仅包含一个 store 实例

单一状态树能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照



## Vue组件中获取state

### 计算属性方式

由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在**计算属性**中返回某个状态

每当 `store.state.count` 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

缺点：在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态

改进：注入store

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

### mapState辅助函数方式

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余

为了解决这个问题，可以使用 `mapState` 辅助函数帮助生成计算属性

#### 对象模式

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    },
      
    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',
  })
}
```

#### 数组模式

当映射的计算属性的名称与 state 的子节点名称相同时，可以给 `mapState` 传一个字符串数组

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

#### 展开运算符

进一步优化：对象展开运算符

```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

