## 概述

由于 Redux-Toolkit 的API设计优秀，且本身是用于 TypeScript 写的，所以对于 RTK 与 TS 集成比较简单



## 标注 initialState 类型

在创建  initialState 时，标注它的类型

```typescript
import { createSlice } from "@reduxjs/redux-toolkit";

// 定义 initialState 的类型
interface DataStateType {
    loading: boolean;
    error: string | null;
    data: any;
}

// 创建 initialState，并标注类型
const initialState: DataStateType = {
    loading: true,
    error: null,
    data: null
}

const dataSlice = createSlice({
    name: "data",
    initialState,
    reducers: {
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
    }
});

export const dataSliceReducer = dataSlice.reducer;
export const dataSliceActions = dataSlice.actions;
```



## 标注 store 和 dispatch 类型

```tsx
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

export type RootState = ReturnType<typeof store.getState>;
export type DispatchType = typeof store.dispatch;
```



## extraReducers的键添加 `.type` 后缀

对于 createAsyncThunk 对应的 extraReducer 的键，需要加上 `.type` 后缀

```typescript
import { createSlice, createAsyncThunk } from "@reduxjs/redux-toolkit";

interface DataStateType {
    loading: boolean;
    error: string | null;
    data: any;
}

const initialState: DataStateType = {
    loading: true,
    error: null,
    data: null
}

export const fetchData = createAsyncThunk(
	"data/fetchData",
    async (id: string, thunkAPI) => {
        const { data } = await axios.get(`url/${id}`);
        return data;
    }
);

export const dataSlice = createSlice({
    name: "data",
    initialState,
    reducers: {},
    extraReducers: {
        // 键需要加上 .type 后缀
        [fetchData.pending.type]: (state, action) => {
            state.loading = true;
        },
        // 键需要加上 .type 后缀
        [fetchData.fullfilled.type]: (state, action) => {
            state.data = action.payload;
            state.loading = false;
        },
        // 键需要加上 .type 后缀
        [fetchData.rejected.type]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
    }
});

export const dataSliceReducer = dataSlice.reducer;
export const dataSliceActions = dataSlice.actions;
```



## 完整示例

```typescript
import { 
   	createSlice, 
	createAsyncThunk, 
    configureStore 
} from "@reduxjs/redux-toolkit";

const initialState: ShoppingCartStateType = {
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
        [getShoppingCart.pending.type]: (state, action) => {
            state.loading = true;
        },
        [getShoppingCart.fullfilled.type]: (state, action) => {
            state.items = action.payload;
            state.loading = false;
        },
        [getShoppingCart.rejected.type]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
        [addShoppingCart.pending.type]: (state, action) => {
            state.loading = true;
        },
        [addShoppingCart.fullfilled.type]: (state, action) => {
            state.items = action.payload;
            state.loading = false;
        },
        [addShoppingCart.rejected.type]: (state, action) => {
            state.loading = false;
            state.error = action.payload;
        },
        [deleteShoppingCart.pending.type]: (state, action) => {
            state.loading = true;
        },
        [deleteShoppingCart.fullfilled.type]: (state, action) => {
            state.items = [];
            state.loading = false;
            state.error = null;
        },
        [deleteShoppingCart.rejected.type]: (state, action) => {
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

