## SFC

### 概念

SFC全称：Single File Components，单文件组件，后缀名为`.vue`

### 背景

在很多 Vue 项目中，我们使用 `app.component` 来定义全局组件，紧接着用 `app.mount('#app')` 在每个页面内指定一个容器元素

这种方式在很多中小规模的项目中运作的很好，在这些项目里 JavaScript 只被用来加强特定的视图，但当在更复杂的项目中，或者你的前端完全由 JavaScript 驱动的时候，下面这些缺点将变得非常明显

* **全局定义 ** 强制要求每个 component 中的命名不得重复
* **字符串模板** 缺乏语法高亮
* **不支持 CSS** 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
* **没有构建步骤** 限制只能使用 HTML 和 ES5 JavaScript，而不能使用预处理器，如 Pug (formerly Jade) 和 Babel

所有这些都可以通过扩展名为 `.vue` 的 **single-file components (单文件组件)** 来解决，并且还可以使用 webpack 或 Browserify 等构建工具

### 简单示例

```vue
<template>
	<p>{{ greeting }} World!</p>
</template>

<script>
export default {
    data() {
        return {
            greeting: "Hello"
        }
    }
}
</script>

<style scoped>
    p {
        font-size: 2em;
        text-align: center;
    }
</style>
```



## Vue CLI

### node环境搭建

* 使用 nvm 安装node
* 使用 nrm 切换npm源

### 安装

```shell
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

### 确认版本号

确认版本号在 `4.5.0`以上

```shell
vue --version
# OR
vue -V
```

### 创建项目

```shell
vue create [项目名]
# OR
vue ui
```
