## 需求

某些情况下，我们希望渲染的内容独立于父组件，甚至是独立于当前挂载到的DOM元素中（默认都是挂载到id为root的DOM元素上的），比如模态框Modal的需求

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的能力

* 第一个参数：ReactElement元素或字符串  
* 第二个参数：一个 DOM 元素

```jsx
ReactDOM.createPortal(element, dom)
```



## 示例

准备开发一个Modal组件，它可以将它的子组件渲染到屏幕的中间位置（使用 render props）

### 步骤一

修改index.html添加新节点

```html
<div id="app"></div>
<div id="modal"></div>
```

### 步骤二

先为这个新节点编写样式

```css
#modal {
    postition: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    background-color: blue;
}
```

### 步骤三

编写组件代码，注意返回值是`React.createPortal`

```jsx
function Modal(props) {
    return ReactDOM.createPortal(
    	props.children,
        document.getElementById("modal")
   );
}
```

### 步骤四

使用Modal组件

```jsx
function Home(props) {
    return (
    	<div>
        	<h2>Home</h2>
            <Modal>
            	<h2>Ttitle</h2>
            </Modal>
        </div>
    );
}
```



## 另外一种实现思路

### 思路

1. 动态创建节点
2. 再次使用ReactDOM进行渲染
3. 关闭时，销毁节点

### 实现

1. 创建新的节点
2. 再次使用ReactDOM进行渲染
3. 关闭时，销毁节点

```jsx
const div = document.createElement("div");
document.body.appendChild(div);
```

```jsx
ReactDOM.render(
	<div>
    	Hello World
    </div>,
    div,
)
```
