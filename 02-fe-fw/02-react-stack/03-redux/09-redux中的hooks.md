## 获取/订阅数据

### 概述

redux中，可以使用 `useSelector` 来获取数据

### 参数

传入一个回调函数，回调函数的参数是 `state`

### 示例代码

```js
const language = useSelector(state => state.language);
const languageList = useSelector(state => state.languageList);
```



## 更新数据

### 概述

redux中，更新数据需要派发action，可以使用 `useDispatch` 获取 `dispatch` 函数，从而派发 action

### 示例代码

```js
const dispatch = useDispatch();

dispatch(addLanguageActionCreator("英语", "English"));
dispatch(changeLanguageActionCreator("法语"));
```

