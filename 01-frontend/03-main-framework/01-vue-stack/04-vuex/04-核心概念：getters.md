## getters作用

### 需求

有时候需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果有多个组件需要用到此属性，那么要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想

### 解决

Vuex 允许我们在 store 中定义“getter”**（可以认为是 store 的计算属性）**



## 定义与访问Getter

### 普通定义与访问

Getter 接受 state 作为其第一个参数，getters作为第二个参数

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: (state, getters) => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

访问

```js
store.getters.doneTodos
```

### 方法式定义与访问

可以通过让 getter 返回一个函数，来实现给 getter 传参

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```

访问

```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```



## mapGetters辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性

### 数组模式

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

### 对象模式

如果想将一个 getter 属性另取一个名字，使用对象形式

```js
...mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```

