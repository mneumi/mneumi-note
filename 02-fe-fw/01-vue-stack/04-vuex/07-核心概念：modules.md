## modules作用

### 需求

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，当应用变得非常复杂时，store 对象就有可能变得相当臃肿

### 解决

为了解决以上问题，Vuex 可以将 store 分割成**模块（module）**

每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```



## 模块状态变更

### getter

1. 在启用 module 的情况下，模块内部的getter的第一个参数是模块内部的state

2. 增加第三个参数 rootState，用于获取根state

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  getters: {
    doubleCount (state, getters, rootState) {
      return state.count * 2
    }
  }
}
```

### mutation

1. 在启用 module 的情况下，模块内部的mutation的第一个参数是模块内部的state

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },
}
```

### action

1. 在启用 module 的情况下，模块中的action的第一个参数context中的state是模块内部state

2. 新增属性`rootState`，用于获取根state

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```



## 命名空间

### 默认情况

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的，这样使得多个模块能够对同一 mutation 或 action 作出响应

### 添加命名空间

如果希望模块具有更高的封装度和复用性，可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块

当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,
      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> store.getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> store.dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> store.commit('account/login')
      },
      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> store.getters['account/profile']
          }
        },
        // 进一步嵌套命名空间
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> store.getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

### 在带有命名空间前提下访问全局内容

#### getter

如果希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为第三和第四参数传入 getter

```js
modules: {
  foo: {
    namespaced: true,
    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    }
  }
}
```

#### action

如果希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为属性注入到 context 中

```js
modules: {
  foo: {
    namespaced: true,
    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

### 在带有命名空间前提下注册全局 action

若需要在带命名空间的模块注册全局 action，可以添加 `root: true`，并将这个 action 的定义放在函数 `handler` 中

```js
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,
      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```

### 命名空间下的辅助函数

#### mapState

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
}
```

进一步简化

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
}
```

#### mapActions

```js
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

进一步简化

```js
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```





## 模块动态注册与卸载

### 注册动态模块

在 store 创建**之后**，可以使用 `store.registerModule` 方法动态注册模块

之后就可以通过 `store.state.myModule` 和 `store.state.nested.myModule` 访问模块的状态

```js
import Vuex from 'vuex'

const store = new Vuex.Store({ /* 选项 */ })

// 注册模块 `myModule`
store.registerModule('myModule', {
  // ...
})
// 注册嵌套模块 `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
```

### 卸载动态模块

可以使用 `store.unregisterModule(moduleName)` 来动态卸载模块

注意，你不能使用此方法卸载静态模块（即创建 store 时声明的模块）

### 检查模块是否已经存在

可以通过 `store.hasModule(moduleName)` 方法检查该模块是否已经被注册到 store