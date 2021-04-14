## 构建项目

```shell
yarn add @vue/cli
```

```shell
vue create column
# 选择 TypeScript
```

选择

* Use class-style component syntax? (y/N)
* Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? (Y/n)
* 



## 插件

ESLint

```js
// 新建 .vscode/setting.json 文件，写入如下配置
{
    "eslint.validate": ["typescript"]
}
```

Vetur



## 定义Component

```typescript
import { defineComponent } from "vue";

export default defineComponent({
    name: "",
    props: {},
    setup(props, context) {
        
    }
})
```





## 泛型

```typescript
const result = ref<T | null>(null);
const loading = ref<boolean>(true);
```



## 定义类型

```typescript
import { PropType } from "vue";

props: {
    list: {
        type: Array as PropType<ColumnProps[]>
    },
    user: {
        type: Object as PropType<UserProps>
    }
}
```

