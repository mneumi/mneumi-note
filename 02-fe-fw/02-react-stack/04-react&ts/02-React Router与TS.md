## 高阶组件HOC

### 类型

RouteComponentProps

### 作用

用于被 withRouter 高阶组件包裹的组件

### 示例代码

#### 基础使用

```tsx
import { RouteComponentProps, withRouter } from "react-router-dom";

const DetailPage: React.FC<RouteComponentProps> = (props) => {
    console.log(props.history);
    console.log(props.location);
    console.log(props.match);
    console.log(props.match.params);
};

export default withRouter(DetailPage);
```

#### 写法1：使用类型继承

```tsx
import React from "react";
import { RouteComponentProps, withRouter } from "react-router-dom";

interface PropsType extends RouteComponentProps {
    id: string;
    imageSrc: string;
};

const ProductImageComponent: React.FC<PropTypes> = ({
    id, imageSrc, history, match, location
}) => {
    return <>
    	<div onClick={() => history.push('/')}>
    		<img src={imageSrc} />
    	</div>
    </>
};

export default withRouter(ProductImageComponent);
```

#### 写法2：使用交叉类型

```tsx
import React from "react";
import { RouteComponentProps, withRouter } from "react-router-dom";

interface PropsType {
    id: string;
    imageSrc: string;
};

const ProductImageComponent: React.FC<PropTypes & RouteComponentProps> = ({
    id, imageSrc, history, match, location
}) => {
    return <>
    	<div onClick={() => history.push('/')}>
    		<img src={imageSrc} />
    	</div>
    </>
};

export default withRouter(ProductImageComponent);
```



## hooks

### 概述

在4个hook中，useRouteMatch和useParams能够与TS配合

### 类型

类型都是定义 params 的类型

### 示例代码

#### useParams

```tsx
import React from 'react';
import { useParams } from 'react-router-dom';

interface RouteParams {
    id: string;
}

export default () => {
    const params = useParams<RouteParams>();
    
    return (
        <div>
            动态路由：{ params.id }
        </div>
    );
}
```

#### useMatch

```tsx
import React from 'react';
import { useMatch } from 'react-router-dom';

interface RouteParams {
    id: string;
}

export default () => {
    const match = useMatch<RouteParams>();
    
    return (
        <div>
            动态路由：{ match.params.id }
        </div>
    );
}
```