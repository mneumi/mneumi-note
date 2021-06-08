## createApp

用于创建 Vue 应用实例对象



## h

渲染函数，具体看 **渲染函数** 一文



## defineComponent

从实现上看，`defineComponent` 只返回传递给它的对象

但从类型而言，返回的值有一个合成类型的构造函数，即用于手动渲染函数、TSX 和 IDE 工具支持

```js
import { defineComponent } from 'vue'

const MyComponent = defineComponent({
    data() {
        return { count: 1 }
    },
    methods: {
        increment() {
            this.count++
        }
    }
})
```



## defineAsyncComponent

创建一个只有在需要时才会加载的异步组件

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
	import('./components/AsyncComponent.vue')
)

app.component('async-component', AsyncComp)
```

当使用**局部注册**时，你也可以直接提供一个返回 `Promise` 的函数

```js
import { createApp, defineAsyncComponent } from 'vue'

createApp({
    // ...
    components: {
        AsyncComponent: defineAsyncComponent(() => import('./components/AsyncComponent.vue'))
    }
})
```



## resolveComponent

> `resolveComponent` 只能在 `render` 或 `setup` 函数中使用

如果在当前应用实例中可用，则允许按名称解析 `component`

返回一个 `Component`。如果没有找到，则返回 `undefined`

```js
const app = Vue.createApp({})
app.component('MyComponent', {
	/* ... */
})
```

```js
import { resolveComponent } from 'vue'
render() {
	const MyComponent = resolveComponent('MyComponent')
}
```



## resolveDynamicComponent

> `resolveDynamicComponent` 只能在 `render` 或 `setup` 函数中使用

允许使用与 `<component :is="">` 相同的机制来解析一个 `component`

返回已解析的 `Component` 或新创建的 `VNode`，其中组件名称作为节点标签。如果找不到 `Component`，将发出警告

```js
import { resolveDynamicComponent } from 'vue'
render () {
	const MyComponent = resolveDynamicComponent('MyComponent')
}
```



##  resolveDirective

> `resolveDirective` 只能在 `render` 或 `setup` 函数中使用

如果在当前应用实例中可用，则允许通过其名称解析一个 `directive`

返回一个 `Directive`。如果没有找到，则返回 `undefined`

```js
const app = Vue.createApp({})
app.directive('highlight', {})
```

```js
import { resolveDirective } from 'vue'
render () {
  const highlightDirective = resolveDirective('highlight')
}
```



##  withDirectives

> `withDirectives` 只能在 `render` 或 `setup` 函数中使用

允许将指令应用于 **VNode**。返回一个包含应用指令的 VNode

```js
import { withDirectives, resolveDirective } from 'vue';

const foo = resolveDirective('foo');
const bar = resolveDirective('bar');

return withDirectives(h('div'), [
    [foo, this.x],
    [bar, this.y]
])
```



##  nextTick

将回调推迟到下一个 DOM 更新周期之后执行。在更改了一些数据以等待 DOM 更新后立即使用它

```js
import { createApp, nextTick } from 'vue'

const app = createApp({
    setup() {
        const message = ref('Hello!')
        const changeMessage = async newMessage => {
            message.value = newMessage
            await nextTick()
            console.log('Now DOM is updated')
        }
	}
})
```



## createRenderer

TODO