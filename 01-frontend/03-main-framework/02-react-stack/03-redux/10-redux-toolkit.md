## 初识RTK

### 概述

Redux Toolkit，简称 RTK，是 Redux 官方推出的工具集

目的是简化 redux 的使用步骤，减少模板代码，降低学习和使用难度

### 特点

默认集成 immer.js：能够直接操作对象，而不需强制实现纯函数

默认集成 redux-thunk：使 redux 能够直接处理异步逻辑

默认集成 devtools：方便对 redux 进行调试

默认集成 reselector：能够缓存读取结果

### 安装依赖

```shell
npm install @reduxjs/toolkit
# OR
yarn add @reduxjs/toolkit
```



## 使用方法

### createSlice

#### 概述

定义redux片段，类似于一个**子reducer**

#### 示例代码

```tsx
// 定义 initialState 的类型
interface ProductDetailState {
    loading: boolean;
    error: string | null;
    data: any;
}

// 创建 initialState
const initialState: ProductDetailState = {
    loading: true,
    error: null,
    data: null
}

export const productDetailSlice = createSlice({
    name: "productDetail", // Slice 的名称
    initialState, // 设置初始 state
    reducers: { // 定义reducer，键为action名称，值为action对应的处理逻辑
        fetchStart: (state) => {
            // return { ...state, loading: true };
            state.loading = true;
        },
        fetchSuccess: (state, action) => {
            state.data = action.payload;
            state.loading = false;
            state.error = null;
        },
        fetchFail: (state, action: PayloadAction<string | null>) => {
            state.loading = false;
            state.error = action.payload;
        }
    },
    extraReducers: {
        [getProductDetail.pending.type]: (state) => {
            state.loading = true;
        },
        [getProductDetail.fullfilled.type]: (state) => {
            state.data = action.payload;
            state.loading = false;
        },
        [getProductDetail.rejected.type]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
    }
});

// 导出该 Slice 的 reducer
export const productDetailSliceReducer = productDetailSlice.reducer;

// 导出该 Slice 的所有 Action
export const productDetailSliceActions = productDetailSlice.actions;
```

### configureStore

#### 概述

用于创建 RTK 模式的 store

#### 示例代码

```tsx
import { productDetailSliceReducer } from "./productDetailSlice.ts";

const rootReducer = {
    productDetail: productDetailSliceReducer
}

const store = configureStore({
    reducer: rootReducer, // 设置 reducer
    middleware: (getDefaultMiddleware) => [...getDefaultMiddleware(), actionLog], // 设置redux中间件
    devTools: true // 开启 devTools
})
```



## createAsyncThunk

### 概述

用于创建异步的action

### 特点

使用 `createAsyncThunk` 创建的 action，会自动往相应的 Slice 的 extractReducers 中注入 `pending`，`fullfilled`和`rejected`三个状态的action

而且在执行该异步操作时，根据执行的进程，会自动触发相应的action

### 示例代码

```ts
import { productDetailSliceActions } from "./productDetailSlice.ts";

export const getProductDetail = createAsyncThunk(
	"productDetail/getProductDetail", // slice名称/action名称
    async (id: string, thunkAPI) => {
        thunkAPI.dispatch(productDetailSliceActions.fetchStart());
        // 同时也会 dispatch([getProductDetail.pending.type]());
        try {
            const { data } = await axios.get(`url/${id}`);
            thunkAPI.dispatch(productDetailSliceActions.fetchSuccess(data));
            // 同时也会 dispatch([getProductDetail.fullfilled.type](data));
        } catch (error) {
            thunkAPI.dispatch(productDetailSliceActions.fetchFail(error));
            // 同时也会 dispatch([getProductDetail.rejected.type](error));
        }
    }
)
```



## 完整示例1



## 完整示例2