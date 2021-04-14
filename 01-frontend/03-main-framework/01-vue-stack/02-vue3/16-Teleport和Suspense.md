## Teleport

### 定义

`Teleport`译作`传送门`，是一种能够将模板在 `DOM` 中的 `Vue app` 移动到其他`DOM`中的技术

非常类似于 `React` 中的 `Portals`

### 使用场景

`Teleport`这种能够将模板渲染到特定的`DOM`的技术最适合用于实现`Modal`或`Toast`的场景

对于模态框，提示框等常用组件，其不应该与组件具有太深耦合，不应该渲染在组件节点中，因为如果`Modal / Toast`嵌套到组件内部，那么`z-index`和`css`样式将变得复杂，且组件本身也多了复杂的逻辑，容易造成干扰，不方便组件的维护

### 使用方法

使用`<Teleport></Teleport>`组件包裹住需要渲染到其他`DOM`的模板

然后通过`Teleport`的`to`属性，指定目标`DOM`即可

```vue
<template>
	<Teleport to="#modal">
    	<div id="center">
    		<h2>This is a modal</h2>
    	</div>
    </Teleport>
</template>
```

```html
<div id="app"></div>
<div id="modal"></div>
```

### 使用例子

#### Modal实现

```vue
<template>
	<Teleport to="#modal">
    	<div id="modal-container" v-if="isOpen">
    		<slot>This is modal</slot>
            <button @click="buttonClick">关闭</button>
    	</div>
    </Teleport>
</template>

<script>
export default {
   props: {
       isOpen: Boolean
   },
   emits: {
		"close-modal": null
   },
   setup(props, context) {
       const buttonClick = () => {
           context.emit("close-modal")
       }

       return {
           buttonClick
       };
   }
};
</script>

<style>
#modal-container {
	width: 200px;
    height: 200px;
    border: 2px solid black;
    background: white;
    position: fixed;
    left: 50%;
    top: 50%;
    transfrom: translate(-50%, -50%);
}
</style>
```

使用示例

```vue
const modalIsOpen = ref(false)
const openModal = () => {
  modalIsOpen.value = true
}
const onModalClose = () => {
  modalIsOpen.value = false
}

<button @click="openModal">Open Modal</button><br/>
<modal :isOpen="modalIsOpen" @close-modal="onModalClose"> My Modal !!!!</modal>
```

#### Toast实现

```vue
<template>
	<button @click="showToast" class="btn">Toast!</button>
	<Teleport to="#Teleport-target">
		<div v-if="visible">
			<div class="toast-msg">我是一个 Toast 文案</div>
		</div>
	</Teleport>
</template>

<script>
export default {
    setup() {
        const visible = ref(false);

        let timer;

        const showToast = () => {
            visible.value = true;

            clearTimeout(timer);

            timer = setTimeout(() => {
                visible.value = false;
            }, 2000);
        };

        return {
            visible,
            showToast
        };
    }
};
</script>
```



## Suspense

### 定义

`Suspense` 会暂停组件渲染，并重现一个回落组件，直到满足一个条件，才会渲染组件

### 使用场景

`Suspense` 组件常常用于渲染异步组件，比如

* 创建 Loading
* 等待 API 回调
* 异步的setup方法

### 注意事项

如果使用 Suspense，需要返回一个 Promise 对象

### 使用方法

`Suspense`组件需要使用插槽，提供了两个插槽，一个名字叫 `#fallback`，一个叫 `#default`

```vue
<!-- AsyncShow.vue -->
<template>
	<h1>
        {{ result }}
    </h1>
</template>

<script>
export default {
    setup() {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve({
                    result: 42
                });
            }, 3000);
        });
    }
}
</script>
```

使用示例

```vue
<Suspense>
    <template #default>
		<AsyncShow />
    </template>
    <template #fallback>
		<h2>Loading</h2>
    </template>
</Suspense>
```
