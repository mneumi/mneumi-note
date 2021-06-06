## 初识ReactQuery

### 概述

| 项       | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 定义     | React Query是一个专门在 React 中使用的请求库，主要用于处理服务端状态 |
| 作用     | React Query能够请求、缓存、自动刷新获取服务器端的数据        |
| 类似方案 | SWR、Apollo Client、RTK Query                                |

### 优势

* 使用通用的 hooks 来处理请求的中间状态（如loading、error）
* 可以将多次相同请求合并
* 优秀的缓存更新和失效策略
* 专门用于处理服务端状态，使 Redux 能够专注于处理客户端状态

### 生态

* 能够兼容 SSR 框架 Next.js
* 能够用于 React Native
* 支持 TypeScript、GraphQL

### 安装

```shell
npm install react-query
# OR
yarn add react-query
```



## 使用方式（查）

### 概述

React Query 中使用 `useQuery` 进行查询，处理的是`GET`请求

### 示例代码

```jsx
import { useQuery } from "react-query";

const App = () => {
    const { data, error, isIdle, isLoading, isError } = useQuery(
        ["userList", { status: 1, page: 1 }], 
        () => axios.get("/users")
    );
    
    if (isLoading) {
        return <div>Loading</div>;
    }
    
    if (isError) {
        return <div>{ error.message }</div>
    }
    
    return (
    	<ul>
        {
        	data.map(user => <li key={user.id}>{ user.name } </li>)        
        }
        </ul>
    );
}
```

### 参数说明

| 参数               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| 第一个参数（必填） | 标识key，唯一标识一个Query<br />该标识可以是字符串或数组，如果是数组，则可以传递参数 |
| 第二个参数（必填） | 用于请求数据的回调函数，必须返回Promise                      |
| 第三个参数（可选） | React Query 的配置信息（见下文）                             |

### 请求特点

* 如果多个组件同时进行了相同的查询（以唯一标识为准），则实际只会发出一个请求（即合并请求）
* 自动缓存和更新请求到的数据
* 自动清理失效的数据



## 使用方式（清除缓存）

### 概述

假设修改了内容，如果不清除缓存，则React Query不会重新发送请求获取最新的数据，所以需要有清除缓存的操作，使用的方法如下

```js
import { queryCache } from "react-query";

queryCache.invalidateQueries("Query对应的标识");
```

### 示例代码

```jsx
import { useQuery, queryCache } from "react-query";

const App = () => {
    const { data, error, isLoading, isError } = useQuery(
        ["userList", { status: 1, page: 1 }], 
        () => axios.get("/users")
    );
    
    if (isLoading) {
        return <div>Loading</div>;
    }
    
    if (isError) {
        return <div>{ error.message }</div>
    }
    
    const refreshData = () => {
        queryCache.invalidateQueryies("userList");
    }
    
    return (
        <>
            <ul>
            {
                data.map(user => <li key={user.id}>{ user.name } </li>)        
            }
            </ul>
            <div onClick={ () => refreshData() }>重新获取数据</div>
        </>
    );
}
```

### 参数说明

`queryCache.invalidateQueries`中传入的是`Query`的唯一标识

如果`Query`的唯一标识是一个数组，也只要传入第一个字符串元素即可



## 使用方式（增删改）

 ### 概述

React Query 中使用 `useMutation` 进行增删改，处理`POST / DELETE / PUT / PATCH` 等方法的请求

### 示例代码

```jsx
import { useQuery, useMutation, queryCache } from "react-query";

const App = () => {
    const { data } = useQuery("user", () => axios.get("/users"));
    
    const { mutate } = useMutation(data => axios.post("/users", data), {
        onSuccess: () => {
            queryCache.invalidateQueries("user"); // 使缓存失效
        }
    });
    
    return (
        <>
            <ul>
            {
                data.map(user => <li key={user.id}>{ user.name }</li>)        
            }
            </ul>
            <div onClick={() => mutate({ name: "mneumi", age: 20 })}>
                新增用户
            </div>
        </>
    );
}
```

### 参数说明

| 参数               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| 第一个参数（必填） | 用于请求数据的回调函数，必须返回Promise                      |
| 第二个参数（必填） | 请求结束后的处理对象，类似于 jQuery 的 $.ajax<br />常用属性有 onSuccess、onError、onSettled |
| 第三个参数（可选） | React Query 的配置信息（见下文）                             |

### 注意事项

在完成对数据的增删改后，需要使React Query的缓存失效



## 使用方式：乐观更新

### 概述 

乐观更新（Optimistic Updated）是一种 UI 行为，意思是客户端在收到服务端响应前就已经把 UI 的状态修改了

因为客户端是**乐观的**，即它认为自己的请求一定能得到服务端对应的响应，所以客户端就直接修改了 UI 状态，以提高用户体验

### 注意事项

UI 的最终状态还是要以服务端响应为准，即就算乐观更新了，但如果最终收到的响应与预期不同，还是要再次修改客户端的 UI 状态

### 示例代码

```jsx
useMatation(
	(params: Partial<Project>) => {
        return client(`projects/${params.id}`, { method: "PATCH", data: params});
    },
    {
        onSuccess: () => queryClient.invalidateQueries("projects"),
        async onMutate(target) {
            // onMutate：立即触发
            const queryKey = ['project', searchParams];
            const previousItem = queryClient.getQueryData(queryKey);
            queryClient.setQueryData(queryKey, (old) => {
                return old.map(project => project.id === target.id ? {
                    ...project, ...target
                } : project) || [];
            });
            return { previousItems }
        },
        onError(error, newItem, context) {
            queryClient.setQueryData(queryKey, context.previousItems);
        }
    }
);
```

通用 hooks

```tsx
export const useConfig = (queryKey: QueryKey, callback: (target: any, old?: any[]) => any[]) => {
    const queryClient = useQueryClient();
    return {
        onSuccess: () => queryClient.invalidateQueries(queryKey),
        async onMutate(target: any) {
            // onMutate：立即触发
            const previousItem = queryClient.getQueryData(queryKey);
            queryClient.setQueryData(queryKey, (old?: any[]) => {
                return callback(target, old);
            });
            return { previousItems }
        },
        onError(error: any, newItem: any, context: any) {
            queryClient.setQueryData(queryKey, context.previousItems);
        }
    }
}

export const useDeleteConfig = (queryKey: QueryKey) => useConfig(queryKey, (target, old) => old?.filter(item => item.id !== target.id) || []);

export const useEditConfig = (queryKey: QueryKey) => useConfig(queryKey, (target, old) => old?.map(item => item.id === target.id ? {...item, ...target} : item) || []);

export const useAddConfig = (queryKey: QueryKey) => useConfig(queryKey, (target, old) => old ? [...old, target] : []);
```



## Provider

```tsx
import { QueryClient, QueryClientProvider, useQuery, useQueryClient } from "react-query";

const queryClient = new QueryClient();

export default function App() {
    return (
    	<QueryClientProvider client={ queryClient }>
        	<Example />
        </QueryClientProvider>
    );
}

function Example() {
    const queryClient = useQueryClient();
    
    queryClient.invalidateQueries("user");
    
    // 获取缓存的数据
    queryClient.getQueryData("user");
    
    // 设置缓存的数据
    queryClient.setQueryData("user", {})
}
```





## React Query配置

### 常用配置选项

| 配置选项             | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| staleTime            | 失效时间，即重新获取数据的时间间隔，单位毫秒，默认0秒        |
| cacheTime            | 数据缓存时间，单位毫秒，默认为 1000 * 60 * 5（5分钟）        |
| retry                | 失败重试次数，默认3次                                        |
| refetchOnWindowFocus | 窗口重新获得焦点时，重新获取数据，默认false                  |
| refetchOnReconnect   | 网络重新连接时，重新获取数据                                 |
| refetchOnMount       | 实例挂载完成时，重新获取数据                                 |
| enable               | 是否主动获取数据，默认为true<br />如果为false，则useQuery不会自动触发，需要使用其他的`refetch`来获取数 |

### 全局配置

```tsx
import { ReactQueryConfigProvider, ReactQueryProviderConfig } from "react-query";

const queryConfig: ReactQueryProviderConfig = {
    queries: {
        refetchOnWindowFocus: true,
        staleTime: 10 * 60 * 1000,
        retry: 2
    }
};

ReactDOM.render(
	<ReactQueryConfigProvider config={queryConfig}>
    	<App />
    </ReactQueryConfigProvider>,
    document.getElementById("root");
);
```

### 单独配置

```tsx
import { useQuery } from "react-query";

const App = () => {
    const { data, error, isIdle, isLoading, isError } = useQuery(
        ["userList", { status: 1, page: 1 }], 
        () => axios.get("/users"),
        { enable: true, staleTime: 1 * 60 * 1000, retry: 1 }
    );
    
    if (isLoading) {
        return <div>Loading</div>;
    }
    
    if (isError) {
        return <div>{ error.message }</div>
    }
    
    return (
    	<ul>
        {
        	data.map(user => <li key={user.id}>{ user.name } </li>)        
        }
        </ul>
    );
}
```

