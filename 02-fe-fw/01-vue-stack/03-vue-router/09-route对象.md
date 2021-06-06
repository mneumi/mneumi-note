## route对象

| 属性       | 说明                                           |
| ---------- | ---------------------------------------------- |
| path       | 路由的路径                                     |
| name       | 路由的名称                                     |
| component  | 路径的组件，值可以是组件也可以是函数（懒加载） |
| components | 命名视图，注意结尾多了个 `s`                   |
| children   | 嵌套路由                                       |
| redirect   | 路由重定向                                     |
| alias      | 路由别名                                       |

```js
const routes = [
    {
        path: "/home",
        name: "Home Route",
        component: Home
    }
    {
    	path: "/about",
    	name: "About Route",
    	component: () => import("../../About.vue")
	}
]
```
