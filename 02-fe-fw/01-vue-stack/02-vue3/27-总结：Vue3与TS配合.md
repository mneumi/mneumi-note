## 环境搭建

### 构建项目

通过webpack

```shell
yarn add @vue/cli

vue create [项目名] # 选择 TypeScript
```

通过vite

```shell
yarn create @vitejs/app [项目名] # 选择 vue -> vue-ts

```

### 插件

ESLint

```js
// 新建 .vscode/setting.json 文件，写入如下配置
{
    "eslint.validate": ["typescript"]
}
```



## 组件定义

### 定义Component

```typescript
import { defineComponent } from "vue";

export default defineComponent({
    name: "",
    props: {},
    setup(props, context) {}
})
```

### 定义props类型

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

### CPA泛型

```typescript
const result = ref<T | null>(null);
const loading = ref<boolean>(true);
```
