## Vue.extend

使用基础 Vue 构造器，创建一个“子类”（即一个新的 Vue 对象）

使用场景：需要一个新的Vue对象时，比如Modal框，实现类似Vue3中的`teleport`组件

参数是一个包含组件选项的对象

```js
// 创建构造器
const Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})

// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```

可以配合 `Vue.component` 使用，`Vue.component`的第二个参数可以是`选项对象`，也可以是 `Vue对象`

```js
// 使用选项对象
Vue.component("child", {
	template: `<h1>{{ first }} {{ second }}</h1>`,
    data() {
        return {
            first: "one",
            second: "two"
        }
    }
});

// 配合Vue.extend
const Child = Vue.extend({
    template: `<h1>{{ first }} {{ second }}</h1>`,
    data() {
        return {
            first: "one",
            second: "two"
        }
    }
});

Vue.comonent("child", Child);
```



## Vue.obserable

让一个对象可响应

Vue 内部会使用它来处理 `data` 函数返回的对象，从而使组件的 `data` 对象是响应式的

```js
const state = Vue.observable({ count: 0 })

const Demo = {
    render(h) {
        return h(
            'button', 
            {
                on: { 
                    click: () => { state.count++ }
                }
            }, 
            `count is: ${state.count}`
        )
    }
}
```



## Vue.compile

将一个模板字符串编译成 render 函数

```js
const res = Vue.compile('<div><span>{{ msg }}</span></div>')

new Vue({
  data: {
    msg: 'hello'
  },
  render: res.render,
  staticRenderFns: res.staticRenderFns
})
```



## Vue.nextTick

在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后立即使用这个方法，**获取更新后的 DOM**

```js
// 修改数据
vm.msg = 'Hello'

// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})

// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
Vue.nextTick()
  .then(function () {
    // DOM 更新了
  })
```



## Vue.set

用于新增或设置属性，且使该属性具有响应式

```js
Vue.set(data, "key", "value")
```



## Vue.delete

删除对象的属性，如果对象是响应式的，确保删除能触发更新视图

```js
Vue.delete(data, "key")
```



## Vue.version

提供字符串形式的 Vue 安装版本号，这对社区的插件和组件来说非常有用

```js
var version = Number(Vue.version.split('.')[0])

if (version === 2) {
  // Vue v2.x.x
} else if (version === 1) {
  // Vue v1.x.x
} else {
  // Unsupported versions of Vue
}
```



## Vue.component

注册全局组件，具体看 **组件化** 一文



## Vue.mixin

全局混入，具体看 **混入** 一文



## Vue.use

使用插件，具体看 **自定义插件** 一文



## Vue.directive

设置全局指令，具体看 **自定义指令** 一文