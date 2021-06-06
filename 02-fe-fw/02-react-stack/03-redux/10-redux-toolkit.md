## 初识RTK

### 概述

Redux Toolkit，简称 RTK，是 Redux 官方推出的工具集

目的是简化 redux 的使用步骤，减少模板代码，降低学习和使用难度

### Redux缺点

* 配置 Redux 的过程过于复杂
* 完整使用 Redux，需要安装比较多的依赖
* 使用 Redux，需要写很多的模板代码

### 特点

| 特点                 | 说明                                   |
| -------------------- | -------------------------------------- |
| 默认集成 immer.js    | 能够直接操作对象，而不需强制实现纯函数 |
| 默认集成 redux-thunk | 使 redux 能够直接处理异步逻辑          |
| 默认集成 devtools    | 方便对 redux 进行调试                  |
| 默认集成 reselector  | 能够缓存读取结果                       |

### 安装依赖

```shell
npm install @reduxjs/toolkit
# OR
yarn add @reduxjs/toolkit
```



## createSlice

### 概述

定义redux片段，类似于一个**子reducer**

### 示例代码

```js
import { createSlice } from "@reduxjs/redux-toolkit";

// 创建 initialState
const initialState = {
    loading: true,
    error: null,
    data: null
}

const dataSlice = createSlice({
    name: "data", // Slice 的名称
    initialState, // 设置初始 state，自动推导出类型
    reducers: { // 定义reducer
        // 键为action的type名称，值为action对应的处理逻辑
        fetchStart: (state) => {
            // return { ...state, loading: true };
            state.loading = true; // 集成了 immer.js，可以直接修改对象
        },
        fetchSuccess: (state, action) => {
            state.data = action.payload;
            state.loading = false;
        },
        fetchFail: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        }
    }
});

export const fetchDataActionCreator = (id) => async (dispatch, getState) => {
	// 开始获取数据
    dispatch(dataSlice.actions.fetchStart());
    
    try {
    	const { data } = await axios.get(`url/${id}`);
        // 获取数据成功
        dispatch(dataSlice.actions.fetchSuccess(data));
    } catch (error) {
    	// 获取数据失败
        dispatch(dataSlice.actions.fetchFail(error));
    }
};

// 导出该 Slice 的 reducer
export const dataSliceReducer = dataSlice.reducer;

// 导出该 Slice 的所有 Action
export const dataSliceActions = dataSlice.actions;
```



## configureStore

### 概述

用于创建 RTK 模式的 store

### 示例代码

```js
import { configureStore } from "@reduxjs/redux-toolkit";
import { dataReducer } from "./dataSlice.js";
import { actionLog } from "./middleware.js";

// 默认支持合并多个reducer
const rootReducer = {
    data: dataSliceReducer
}

const store = configureStore({
    reducer: rootReducer, // 设置根 reducer
    // 设置redux中间件，注意获取默认集成的中间件
    middleware: (getDefaultMiddleware) => [...getDefaultMiddleware(), actionLog], 
    devTools: true // 开启 devTools
});
```



## createAsyncThunk

### 概述

用于创建异步Action函数

### 特点

使用 `createAsyncThunk` 创建的 `thunkAction`，会在执行该异步操作时，根据执行的阶段 `pending / fullfilled / rejected`，自动触发对应的 Action

因为是根据**命名空间**进行匹配的，所以命名空间一定要写对**（要对应 createSlice 中的 name 属性）**

### 区别

`createAsyncThunk`与手动写`异步Action函数`的区别如下：

* `createAsyncThunk` 创建的 `异步Action` 会在异步的三个阶段`pending / fullfilled / rejected` 自动派发相应的 Action
* 手动写异步Action函数则需要自己手动触发三个阶段的Action

### 示例代码

```js
import { createSlice, createAsyncThunk } from "@reduxjs/redux-toolkit";

const initialState = {
    loading: true,
    error: null,
    data: null
};

export const fetchData = createAsyncThunk(
	"data/fetchData", // 命名空间设置：slice名称/action名称
    async (id: string, thunkAPI) => {
        /*
         * 开始获取数据
         * 会自动 thunkAPI.dispatch([fetchData.pending]())
         * 不需要 thunkAPI.dispatch(dataSlice.actions.fetchStart())
         */
        const { data } = await axios.get(`url/${id}`);
        
        /*
         * 获取数据成功，return 数据
         * 会自动 thunkAPI.dispatch([fetchData.fullfilled](data))
         * 不需要 thunkAPI.dispatch(dataSlice.actions.fetchSuccess(data))
         */
        return data;
        
        /*
         * 如果获取数据的过程中发生了错误
         * 会自动 thunkAPI.dispatch([fetchData.rejected](error))
         * 不需要 thunkAPI.dispatch(dataSlice.actions.fetchFail(error))
         */
    }
);

export const dataSlice = createSlice({
    name: "data",
    initialState,
    reducers: {
        // 这里的3个Action实际上并没有作用，但为了对比 extraReducers，还是保留了
        fetchStart: (state) => {
            state.loading = true;
        },
        fetchSuccess: (state, action) => {
            state.data = action.payload;
            state.loading = false;
        },
        fetchFail: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        }
    },
    extraReducers: {
        [fetchData.pending]: (state, action) => {
            state.loading = true;
        },
        [fetchData.fullfilled]: (state, action) => {
            state.data = action.payload;
            state.loading = false;
        },
        [fetchData.rejected]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
    }
});

// 下面这个手动异步Action函数其实已经没用，但为了与 createAsyncThunk 对比，保留下来
export const fetchDataActionCreator = (id) => async (dispatch, getState) => {
	// 开始获取数据
    dispatch(dataSlice.actions.fetchStart());
    
    try {
    	const { data } = await axios.get(`url/${id}`);
        // 获取数据成功
        dispatch(dataSlice.actions.fetchSuccess(data));
    } catch (error) {
    	// 获取数据失败
        dispatch(dataSlice.actions.fetchFail(error));
    }
};

// 导出该 Slice 的 reducer
export const dataSliceReducer = dataSlice.reducer;

// 导出该 Slice 的所有 Action
export const dataSliceActions = dataSlice.actions;
```



## 完整示例

```js
import { 
   	createSlice, 
	createAsyncThunk, 
    configureStore 
} from "@reduxjs/redux-toolkit";

const initialState = {
    loading: true,
    error: null,
    items: []
}

export const getShoppingCart = createAsyncThunk(
	"shoppingCart/getShoppingCart",
    async (thunkAPI) => {
        const { data } = await axios.get(`http://localhost/api/shoppingCart`);
        return data.shoppingCartItems;
    }
);

export const addShoppingCart = createAsyncThunk(
	"shoppingCart/addShoppingCart",
    async (parameters: { productId: string }, thunkAPI) => {
        const { data } = await axios.post(
        	`http://localhost/api/shoppingCart`,
            {
            	productId: parameters.productId 
            }
        );
        return data.shoppingCartItems;
    }
);

export const deleteShoppingCart = createAsyncThunk(
	"shoppingCart/deleteShoppingCart",
    async (parameters: { productId: string }, thunkAPI) => {
        const { data } = await axios.delete(
        	`http://localhost/api/shoppingCart/${parameters.productId}`
        );
        return data.shoppingCartItems;
    }
);

export const shoppingCartSlice = createSlice({
    name: "shoppingCart",
    initialState,
    reducers: {},
    extraReducers: {
        [getShoppingCart.pending]: (state, action) => {
            state.loading = true;
        },
        [getShoppingCart.fullfilled]: (state, action) => {
            state.items = action.payload;
            state.loading = false;
        },
        [getShoppingCart.rejected]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
        [addShoppingCart.pending]: (state, action) => {
            state.loading = true;
        },
        [addShoppingCart.fullfilled]: (state, action) => {
            state.items = action.payload;
            state.loading = false;
        },
        [addShoppingCart.rejected]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
        [deleteShoppingCart.pending]: (state, action) => {
            state.loading = true;
        },
        [deleteShoppingCart.fullfilled]: (state, action) => {
            state.items = [];
            state.loading = false;
            state.error = null;
        },
        [deleteShoppingCart.rejected]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        }
    }
});

const rootReducer = {
    shoppingCart: shoppingCartSlice.reducer
};

const store = configureStore({
    reducer: rootReducer,
    middleware: (getDefaultMiddleware) => [...getDefaultMiddleware(), actionLog], 
    devTools: true
});
```

使用store

```js
const dispatch = useDispatch();

dispatch(getShoppingCart());
dispatch(addShoppingCart({ productId: "abcd" }));
dispatch(deleteShoppingCart({ productId: "abcd" }));
```