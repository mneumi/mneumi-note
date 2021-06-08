## 概念

| 概念                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Render Props是什么     | Render Props是一种特殊的组件，它本身只提供数据和状态，却不决定组件如何渲染 |
| Render Props作用       | 提供一种组件复用的能力                                       |
| Render Props使用方法   | 使用Render Props组件时，需要向Render Props提供一个渲染函数<br />渲染函数参数：Render Props提供的数据和状态<br />渲染函数返回值：使用数据和状态，描述如何渲染组件 |
| Render Props的使用案例 | Context，React Router                                        |

Render Props 的核心思想是：**通过一个函数将class组件的state作为props传递给纯函数组件**



## 使用案例

### 定义Render Props组件

```jsx
class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      x: 0,
      y: 0
    };
  }

  render() {
    const renderFunc = this.props.children; // 获取渲染函数
                                            // 注意，如果只有一个子组件
                                            // 则children不是数组，而是该元素

    return (
      <div style={{ height: '100%' }}
        onMouseMove={(e) => this.handleMouseMove(e)}
      >
        {renderFunc(this.state.x, this.state.y)} {/* 调用渲染函数渲染组件 */}
      </div>
    );
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }
}
```

### 使用Render Props组件

```jsx
function App(props) {
  return (
    <div style={{ height: "100%" }}>
      <Mouse>
        {
          (x, y) => <h1>鼠标位置为 ({x}, {y})</h1>
        }
      </Mouse>
    </div>
  );
}
```

全局样式设置

```css
* {
    margin: 0;
    padding: 0;
}

html,
body {
    height: 100%;
}
```



## Render Props与高阶组件的区别

| 复用方式     | 渲染区别                           | 使用区别     |
| ------------ | ---------------------------------- | ------------ |
| Render Props | 自身没有控制权，不决定组件如何渲染 | 把数据扔出来 |
| 高阶组件     | 自身有控制权，决定组件如何渲染     | 把组件扔进去 |
