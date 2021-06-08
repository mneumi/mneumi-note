## 安装依赖

```shell
npm install @types/redux @types/react-redux @types/redux-thunk --save-dev
# OR
yarn add @types/redux @types/react-redux @types/redux-thunk -D
```



## 创建过程

### ActionType常量

注意：这里不能使用 `:string`显式声明 ，因为如果使用`:string`显式声明，则类型为`string`，而不是字符串字面量了

```typescript
// action_type.ts

// 下面是错误做法
export const FETCH_DATA_START: string = "FETCH_DATA_START";     // 类型：string
export const FETCH_DATA_SUCCESS: string = "FETCH_DATA_SUCCESS"; // 类型：string
export const FETCH_DATA_ERROR: string = "FETCH_DATA_ERROR";     // 类型：string

// 下面是正确做法
export const FETCH_DATA_START = "FETCH_DATA_START";     // 类型："FETCH_DATA_START"
export const FETCH_DATA_SUCCESS = "FETCH_DATA_SUCCESS"; // 类型："FETCH_DATA_SUCCESS"
export const FETCH_DATA_ERROR = "FETCH_DATA_ERROR";     // 类型："FETCH_DATA_ERROR"
```

### ActionCreator

```typescript
// action_creator.ts
import { 
    FETCH_DATA_START, 
    FETCH_DATA_SUCCESS,
    FETCH_DATA_ERROR
} from "./action_type.ts";

interface fetchDataStartActionType {
    type: typeof FETCH_DATA_START;
}

interface fetchDataSuccessActionType {
    type: typeof FETCH_DATA_SUCCESS;
    payload: {
        list: string[];
    };
}

interface fetchDataErrorActionType {
    type: typeof FETCH_DATA_ERROR;
    payload: string | null;
}

export type ActionType =
  | fetchDataStartActionType
  | fetchDataSuccessActionType
  | fetchDataErrorActionType;

export const fetchDataStartActionCreator = (): fetchDataStartActionType => {
    return { 
        type: FETCH_DATA_START,
    };
}

export const fetchDataSuccessActionCreator = (data: {
  list: string[];
}): fetchDataSuccessActionType => {
  return {
    type: FETCH_DATA_SUCCESS,
    payload: data,
  };
};

export const fetchDataErrorActionCreator = (
  error: string | null
): fetchDataErrorActionType => {
  return {
    type: FETCH_DATA_ERROR,
    payload: error,
  };
};
```

### 定义Reducer

```typescript
// reducer.ts
import { 
    FETCH_DATA_START, 
    FETCH_DATA_SUCCESS,
    FETCH_DATA_ERROR
} from "./action_type.ts";
import { ActionType } from "./action_creator.ts";

interface initialStateType {
    loading: boolean;
    error: string | null;
    list: string[];
}

const initialState: initialStateType = {
    loading: false,
    error: null,
    list: [],
}

export const reducer = (
  state = initialState,
  action: ActionType
): initialStateType => {
  switch (action.type) {
    case FETCH_DATA_START:
      return { ...state, loading: true };
    case FETCH_DATA_SUCCESS:
      return { ...state, loading: false, list: action.payload.list };
    case FETCH_DATA_ERROR:
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
};
```

### 定义中间件

```typescript
// middleware.ts
import { Middleware } from "redux";

export const actionLog: Middleware = (store) => (next) => (action) => {
    console.log("state 当前: ", store.getState());
   	console.log("fire action: ", action);
    next(action);
    console.log("state 更新: ", store.getState());
}
```

### 定义Store

```typescript
// store.ts
import { createStore, applyMiddleware } from "redux";
import { actionLog } from "./middleware.ts";

export const store = createStore(reducer, applyMiddleware(thunk));

export type RootState = ReturnType<typeof store.getState>;
export type DispatchType = typeof store.dispatch;
```

### 异步Action

```typescript
// action_creator.ts
import { ThunkAction } from "redux-thunk";

export const fetchDataActionCreator =
  (): ThunkAction<
    void, // R：Return，当前函数的返回值
    RootState, // S: Store，Store的类型
    unknown, // E: ExtraArgument，额外参数的类型
    ActionType // A: Action，涉及到的Action的类型
  > =>
  async (dispatch, getState) => {
    dispatch(fetchDataStartActionCreator());
    try {
      const data = await axios.get('http://www.demo.com');
      dispatch(fetchDataSuccessActionCreator(data));
    } catch (error) {
      dispatch(fetchDataErrorActionCreator(error.message));
    }
  };
```



## 使用方式 - HOC

### mapStateToProps

```typescript
// Home.jsx
import { RootState } from "./store.ts";

const mapStateToProps = (state: RootState) => {
  return {
    list: state.list,
  };
};
```

### mapDispatchToProps

注意此处 dispatch 的类型，如果只有同步Action，则引用 `redux` 中的 `Dispatch类型`

```typescript
import { Dispatch } from "redux";
```

如果含有thunk action，则引用 `redux-thunk` 中的 `ThunkDispatch` 类型

```typescript
import { ThunkDispatch } from "react-redux";
```

示例代码

```typescript
// Home.jsx
// import { Dispatch } from "redux";
import { ThunkDispatch } from "react-redux";
import { RootState } from "./store.ts";
import { ActionType, fetchDataActionCreator } from "./action_creator.ts";

const mapDispatchToProps = (
  dispatch: ThunkDispatch<
    RootState, // S: Store，Store的类型
    unknown, // E: ExtraArgument, ExtraArgument的类型
    ActionType // A: Action，Action的类型
  >
) => {
  return {
    fetchData: () => dispatch(fetchDataActionCreator()),
  };
};
```

### connect

```tsx
// Home.tsx
import { connect } from "react-redux";

type PropsType = ReturnType<typeof mapStateToProps> &
  ReturnType<typeof mapDispatchToProps>;

class HomeComponent extends React.Component<PropsType> {
  constructor(props) {
    super(props);
  }

  render() {
    console.log(this.props.list);
    console.log(this.props.fetchData);
    return <div>Home</div>;
  }
}

export const Home = connect(
	mapStateToProps,
    mapDispatchToProps
)(HomeComponent);
```



## 使用方式 - HOOKS

### useSelector

#### 写法1：原始版

```typescript
import { useSelector } from "react-redux";

const list = useSelector((state: RootState) => state.list);
```

#### 写法2：封装版

```typescript
import {
    useSelector as useReduxSelector,
    TypedUseSelectorHook
} from "react-redux";

import { RootState } from "./store";

export const useSelector: TypedUseSelectorHook<RootState> = useReduxSelector;

const list = useSelector(state => state.list);
```

### useDispatch

注意此处 dispatch 的类型，如果只有同步Action，则引用 `redux` 中的 `Dispatch类型`

```typescript
import { Dispatch } from "redux";
```

如果含有thunk action，则引用 `redux-thunk` 中的 `ThunkDispatch` 类型

```typescript
import { ThunkDispatch } from "react-redux";
```

示例代码

```typescript
import { useDispatch, ThunkDispatch } from "react-redux";
import { RootState } from "./store.ts";
import { ActionType } from "./action_creator.ts";

const dispatch =
  useDispatch<
    ThunkDispatch<
      RootState, // S: Store，Store的类型
      unknown, // E: ExtraArgument, ExtraArgument的类型
      ActionType // A: Action，Action的类型
    >
  >();

dispatch(fetchDataActionCreator());
```

