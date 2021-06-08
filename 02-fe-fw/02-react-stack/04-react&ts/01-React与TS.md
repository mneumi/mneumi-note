## 引入TypeScript

### 新项目

```shell
npx create-react-app [项目名] --template typescript
```

### 已有项目

安装下列依赖

```shell
npm install typescript @types/node @types/react @types/react-dom @types/jest --save-dev
# OR
yarn add typescript @types/node @types/react @types/react-dom @types/jest -D
```



## 类组件与TS

### 类型

`React.Component<Props, State> / React.PureComponent<Props, State>`

### 示例代码

```tsx
interface Props {
    id: number;
    title: string;
    onClick: () => void;
}

interface State {
    show: boolean;
}

class Card extends React.Component<Props, State> {
    constructor(props) {
        super(props);
        this.state = {
            show: true;
        }
    }
    
    render() {
        return (
         this.state.show ?
         <div id={this.props.id}>
        	<span>{this.props.title}</span>
            <button onClick={() => this.props.onClick()}>点击</button>
        </div>) : <div></div>;
    }
}
```

```tsx
interface Props {
    id: number;
    title: string;
    onClick: () => void;
}

interface State {
    show: boolean;
}

class Card extends React.PureComponent<Props, State> {
    constructor(props) {
        super(props);
        this.state = {
            show: true;
        }
    }
    
    render() {
        return (
         this.state.show ?
         <div id={this.props.id}>
        	<span>{this.props.title}</span>
            <button onClick={() => this.props.onClick()}>点击</button>
        </div>) : <div></div>;
    }
}
```



## 函数式组件与TS

### 类型

`React.FC<Props> / React.FunctionComponent<Props>`

### 示例代码

```tsx
import React from "react";

interface UserProps {
  id: string;
  name: string;
  email: string;
}

const User: React.FC<UserProps> = (props) => {
  const { id, name, email } = props;
  
  return <>
    <div>{id}</div>
    <div>{name}</div>
    <div>{email}</div>
  </>;
}

export default User;
```

```tsx
import React from "react";

interface UserProps {
  id: string;
  name: string;
  email: string;
}

const User: React.FunctionComponent<UserProps> = (props) => {
  const { id, name, email } = props;
  
  return <>
    <div>{id}</div>
    <div>{name}</div>
    <div>{email}</div>
  </>;
}

export default User;
```



## hooks与TS

### useState

```tsx
const [username, setUsername] = useState<string>("");
const [loading, setLoading] = useState<boolean>(false);
```

实际使用过程中，可以使用自动推断

```typescript
useState<S>(initialState: S | (() => S));

在使用时，可以不用 useState<类型>(类型变量)，而直接 useState(类型变量)

编译器可以由参数类型，反向自动推导出泛型的类型
```



## props合并类型

### 概述

| 合并类型              | 说明               |
| --------------------- | ------------------ |
| 方法1：使用 extends   | 不适合多个类型合并 |
| 方法2：使用交叉类型 & | 适合多个类型合并   |

### 使用心得

可以使用 extends 来组合第三方库的组件类型

```tsx
import { Table, TableProps } from "antd";

interface ListProps extends TableProps<Project> {
    users: User[];
}

// 解构某个属性出来，同时将别的属性放到 props 这个变量中
export const List= ({ users, ...props}: ListProps) => {
    return <Table {...props} />
}
```

使用示例

```tsx
<List loading={isLoading} users={users} dataSource={list} />
```



## React工具类型

### React.ComponentType

用于提取组件的 Props 类型，常用于配合高阶组件使用

```tsx
import React from "react";

const withEnhanceUser = (ChildComponent: React.ComponentType<UserProps>) => {
    return (props) => {
        return (
        	<ChildComponent {...props} />
        );
    }
}
```

### React.PropsWithChildren\<T>

一种语法糖写法，常用于含有`children`的props

```tsx
import React from "react";

interface Props {
    name: string;
    age: number;
    children: React.ReactNode;
};

const User: React.FC<Props> = (props) => {
    const { name, age, children } = props;
    
    return <div>
    	<div>name: { name }</div>
        <div>age: { age }</div>
        { children }
    </div>;
};
```

等价于下列的语法糖写法

```tsx
import React from "react";

interface Props {
    name: string;
    age: number;
};

const User: React.FC<React.PropsWithChildren<Props>> = (props) => {
    const { name, age, children } = props;
    
    return <div>
    	<div>name: { name }</div>
        <div>age: { age }</div>
        { children }
    </div>;
};
```



## React内置类型

| 内置类型           | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| React.ReactNode    | children类型<br />（联合类型：string \| number \| boolean \| ReactElement \| ReactNodeArray） |
| React.ReactElement | 一个接口，包含`type`,`props`,`key`三个属性值，该类型的变量值只能是两种： `null` 和 `ReactElement`实例 |
| JSX.Element        | `JSX.Element`是`ReactElement`的子类型，两者几乎一致          |

关系对比：`JSX.Element` ≈ `ReactElement` ⊂ `ReactNode`