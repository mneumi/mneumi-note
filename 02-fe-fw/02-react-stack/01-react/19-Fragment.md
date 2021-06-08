## 需求

在之前的使用中，由于`JSX`要求只有一个根组件，所以总是在一个组件中返回内容时包裹一个`div`元素作为根标签，它会被渲染出来

```jsx
function App() {
    return (
    	<div> {/* 作为根组件，会进行渲染 */}
        	<h2>Hello World</h2>
            <h2>Hello React</h2>
        </div>
    );
}
```

现在，希望可以有根标签，但又不想渲染这样一个div应该如何操作呢？

可以使用Fragment组件，Fragment 组件允许你将子列表分组，而无需向 DOM 添加额外节点

```jsx
import { Fragment } from "react";

function App() {
    return (
    	<Fragment> {/* 作为根组件，不会进行渲染 */}
        	<h2>Hello World</h2>
            <h2>Hello React</h2>
        </Fragment>
    );
}
```



## 使用场景

### 场景1：作为根标签

```jsx
import { Fragment } from "react";

function App() {
    return (
    	<Fragment> {/* 作为根组件，不会进行渲染 */}
        	<h2>Hello World</h2>
            <h2>Hello React</h2>
        </Fragment>
    );
}
```

### 场景2：想占位又不像被渲染

Fragment不止可以用在根标签，可以用在任何需要有标签，但又不像进行渲染的地方

类似于 Vue 中的 `template` 标签

```jsx
<div>
    {
        this.state.friends.map((item,index)=>{
            return (
                <Fragment>
                    <div>{item.name}</div>
                    <div>{itme.age}</div>
                    <hr />
                </Fragment>
            );
        })
    }
</div>
```



## 短语法写法

Fragment组件提供了简化写法

```jsx
<Fragment></Fragment>
// 可以写成
<></>
```

示例

```jsx
import { Fragment } from "react";

function App() {
    return (
    	<> {/* 作为根组件，不会进行渲染 */}
        	<h2>Hello World</h2>
            <h2>Hello React</h2>
        </>
    );
}
```

注意事项：

* 短语法不能添加任何属性
* 而Fragment可以添加属性，比如添加 key
