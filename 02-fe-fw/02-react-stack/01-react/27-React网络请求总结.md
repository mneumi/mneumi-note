## 类组件中的网络请求

```jsx
import React from "react";

class Home extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            loading: false,
            error: null;
            data: null;
        }
    }
    
    componentDidMounted() {
        // 开启 loading
        this.setState({
            ...this.state,
            loading: true
        });
        
        // 开始请求数据
        axios.get("http://www.demo.com")
        	.then(resp => {
            	// 请求成功，设置data，取消loading
            	this.setState({
                    ...this.state,
                    data: resp.data,
                    loading: false
                });
        	})
        	.catch(error => {
            	// 请求失败，设置error，取消loading
            	this.setState({
                    ...this.data,
                    error: error.message,
                    loading: false
                })
        	});
    }
    
    render() {
        return <div>{ this.state.data }</div>
    }
}
```



## 函数式组件中的网络请求

```js
import React, { useState, useEffect } from "react";

const Home = (props) => {
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);
    const [data, setData] = useState(null);
    
    useEffect(() => {
        async fetchData = () => {
            setLoading(true); // 请求开始，开启loading
            
            try {
                const { data } = await axios.get("http://www.demo.com");
                setData(data); // 请求成功，设置data
            } catch (error) {
                setError(error); // 请求失败，设置error
            } finally {
                setLoading(false); // 无论请求是否成功，都取消loading
            }
        }
        
        fetchData();
    }, []);
}
```



## Redux中的网络请求

```js
import { createStore } from "redux";

const FETCH_DATA_START = "FETCH_DATA_START";
const FETCH_DATA_SUCCESS = "FETCH_DATA_SUCCESS";
const FETCH_DATA_ERROR = "FETCH_DATA_ERROR";

const fetchDataStartActionCreator = () => {
    return { type: FETCH_DATA_START };
}

const fetchDataSuccessActionCreator = (data) => {
  return {
    type: FETCH_DATA_SUCCESS,
    payload: data,
  };
};

const fetchDataErrorActionCreator = () => {
  return {
    type: FETCH_DATA_ERROR,
    payload: error,
  };
};

const fetchDataActionCreator = () => {
  return async (dispatch, getState) => {
    // 请求开始，设置 loading
    dispatch(fetchDataStartActionCreator());
    try {
      const { data } = await axios.get('http://www.demo.com');
      // 请求成功，设置 data，关闭 loading
      dispatch(fetchDataSuccessActionCreator(data));
    } catch (error) {
      // 请求失败，设置 error，关闭 loading
      dispatch(fetchDataErrorActionCreator(error.message));
    }
  };
}

const initialState = {
    loading: false,
    error: null,
    data: null,
}
    
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_DATA_START:
      // 请求开始，设置 loading
      return { ...state, loading: true };
    case FETCH_DATA_SUCCESS:
      // 请求成功，设置 data，关闭 loading
      return { ...state, loading: false, data: action.payload };
    case FETCH_DATA_ERROR:
      // 请求失败，设置 error，关闭 loading
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
};

const store = createStore(reducer, applyMiddleware(thunk));
```



## RTK中的网络请求

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



## React-Query

