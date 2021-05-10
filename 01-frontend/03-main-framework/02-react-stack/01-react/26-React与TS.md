## 引入TypeScript

### 创建新项目

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

React.Component<Props, State>

```tsx
class User extends React.Component
```





## 函数式组件与TS

### React.FC<Props> / React.FunctionComponent<Props>

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

