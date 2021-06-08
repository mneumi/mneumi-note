## 请求一次的hooks

```tsx
export const useMount = (callback: () => void) => {
    useEffect(() => {
        callback();
    }, []);
};
```



## 防抖hooks

React版debounce，针对的是值，由于值是debounce的，所以依赖该值的函数会被debounce

```tsx
export const useDebounce = <V>(value: V, delay?: number) => {
    const [debounceValue, setDebounceValue] = useState(value);
    
    useEffect(() => {
        const timeout = setTimeout(() => {
            setDebounceValue(value)
        }, delay);
        
        return () => clearTimeout(timeout);
    }, [value, delay]);
    
    return debounceValue;
}
```



## useTitle

有问题，但通过闭包恰好解决的写法

```tsx
export const useDocumentTitle = (title: string, keepOnUnmount: boolean = true) => {
    const oldTitle = document.title; // 这里 oldTitle 实际每次重新渲染都会改变，第二次渲染时，就无法记录真正的oldTitle
    
    useEffect(() => {
        document.title = title;
    }, [title]);
    
    useEffect(() => {
        return () => {
            if (!keepOnUnmount) {
                document.title = oldTitle; // 通过闭包，恰好能够记录第一次的，真正的oldTitle
            }
        }
    }, [keepOnUnmount, oldTitle]);
}
```

正确写法

```tsx
export const useDocumentTitle = (title: string, keepOnUnmount: boolean = true) => {
    const oldTitle = useRef(document.title).current;
    
    useEffect(() => {
        document.title = title;
    }, [title]);
    
    useEffect(() => {
        return () => {
            if (!keepOnUnmount) {
                document.title = oldTitle;
            }
        }
    }, [keepOnUnmount, oldTitle]);
}
```

另一种不优雅写法

```tsx
 const [oldTitle, setOldTitle] = useState('');
  useEffect(() => {
    setOldTitle(document.title)
    console.log("oldTitle", oldTitle || 'not done')

  }, [])
  useEffect(() => {
    console.log("start", document.title || 'undefined')

    document.title = title;
    return () => {
      console.log("oldTitle", oldTitle);
      if (!keepOnceUnmounted) {
        document.title = oldTitle;
      }
    }
  }, [keepOnceUnmounted,  title])
```



## useAsync

### 概述

使用高级Hook - useAsync 统一处理Loading和Error状态

### 定义

```tsx
interface State<D> {
    error: Error | null;
    data: D | null;
    stat: 'idle' | 'loading' | 'error' | 'success'
}

const defaultInitialState: State<null> = {
    stat: 'idle',
    data: null,
    error: null
}

export const useAsync = <D>(initialState?: State<D>) => {
    const [state, setState] = useState<State<D>>({
        ...defaultInitialState,
        ...initialState
    });
              
    const setData = (data: D) => setState({
        data,
        stat: "success",
        error: null
    });
              
    const setError = (error: Error) => setState({
        error,
        stat: "error",
        data: null
    });
              
    // run 用来触发异步请求
    const run = (promise: Promise<D>) => {
        if (!promise || !promise.then) {
            throw new Error("请传入 Promise 类型数据");
        }
        setState({...state, stat: 'loading'});
        return pormise.then(data => {
            setData(data);
            return data;
        }).catch(error => {
            setError(error);
            return error;
        })
    }
    
    return {
        isIdle: state.stat === 'idle',
        isLoading: state.stat === 'loading',
        isError: state.stat === 'error',
        isSuccess: state.stat === 'success'
        run,
        setData,
        setError,
        ...state
    }
}
```

### 使用

```tsx
const { run, isLoading, error, data: list } = useAsync<Project[]>();

useEffect(() => {
    run(client("projects", { data: cleanObject(debouncedParam) }))
}，[debouncedParam]);

return (
	<Container>
    	{ error ? <Typeograhy.Text type={"danger"}}>{error.message}</Typeograhy.Text> : null }
        <List loading={isLoading} users={users} dataSource={list || []} />
    </Container>
);
```
