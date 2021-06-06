::: tip

此为 Vue3 补充的 Vue Router，只关注新用法

Vue Router的主要用法参考**Vue Router 模块**

:::



## 添加依赖

```shell
yarn add vue-router
```



## 创建路由对象

```js
import { createRouter, createWebHashHistory } from "vue-router";

const routes = [
    {
        path: "/",
        name: "Home",
        component: Home
    },
    {
        path: "/about",
        name: "About",
        component: () => import(/* webpackChunkName: "about" */ "../views/About.vue")
    }
]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router;
```



## 注册路由插件

```js
createApp(App).use(router).mount("#app")
```



## 使用路由组件

```html
<router-link to="">Home</router-link>
<router-view />
```



## composition API中的VueRouter

### API

useRoute：获取当前路由信息

useRouter：获取路由器对象

### 示例代码

```js
import { useRoute, useRouter } from 'vue-router';

export default defineComponent({
    setup() {
        const router = useRouter();
        
        const onFormSUbmit = (result: boolean) => {
            if (result) {
                router.push({name: "column", params: {id: 1}})
                router.push(`/column/1`)
            }
        }

        return {
            route
        };
    }
});
```

