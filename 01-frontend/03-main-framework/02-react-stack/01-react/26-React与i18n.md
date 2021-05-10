## 概述

### I18n含义

I18n指的是：网站国际化 `Internationalizatoin`

### 原理

语言包作为静态资源文件单独保存：xml，json

每种语言对应一个文件，切换语言设置时，语言文件也会随之切换



## React中的i18n工具

* i18next：目前最主流的框架

* react-i18next：i18next的React插件

* 文档：https://react.i18next.com

* 安装依赖

  ```shell
  npm install react-i18next i18next --save
  # OR
  yarn add react-i18next i18next
  ```




## 初始化react-i18n

### 创建语言文件

##### zh.json

```json
{
    "home_page": {
        "hot_recommended": "爆款推荐",
        "new_product": "新品上市"
    },
    "footer": {
        "detail": "版权所有 @ React"
    }
}
```

##### en.json

```json
{
    "home_page": {
        "hot_recommended": "Hot Recommended",
        "new_product": "New Proudct"
    },
    "footer": {
        "detail": "Copyright @ React"
    }
}
```

### 配置react-i18next

```typescript
// config.ts
import i18n from "i18next";
import { initReactI18next } from "react-i18next";

import translation_en from "./en.json";
import translation_zh from "./zh.json";

const resources = {
    en: {
    	translation: translation_en  
    },
    zh: {
        translation: translation_zh
    }
}

i18n
    .use(initReactI18next)
    .init({
        resources,
        lng: "zh",
        interpolation: {
            escapedValue: false // React已经默认防止XSS攻击
        }
    })
```

### 引入react-i18next

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import "./i18n/config.ts"; // 导入后，即已经完成对 react-i18n 的集成了

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```



## 使用 react-i18n

### 通过高阶组件

```tsx
import { withTranslation, WithTranslation } from "react-18next";

const HomePageComponent: React.FC<WithTranslation> = (props) => {
    const { t } = props;
    
    return <>
    	<span>{ t("home_page.hot_recommended") }</span>
    	<span>{ t("home_page.new_product") }</span>
    	<span>{ t("footer.detail") }</span>
    </>
}

export default withTranslation()(HomePageComponent);
```

### 通过hooks

```tsx
import { useTranslation } from "react-18next";

const HomePageComponent: React.FC<WithTranslation> = (props) => {
    const [t, i18n] = useTranslation();
    
    return <>
    	<span>{ t("home_page.hot_recommended") }</span>
    	<span>{ t("home_page.new_product") }</span>
    	<span>{ t("footer.detail") }</span>
    </>
}

export default withTranslation()(HomePageComponent);
```



## 集成redux

### 配置action constanst

```tsx
export const CHANGE_LANGUAGE: string = "change_language";
export const ADD_LANGUAGE: string = "add_language";
```

### 配置action creator

```tsx
interface ChangeLanguageAction {
    type: typeof CHANGE_LANGUAGE,
    payload: "zh" | "en"
}

interface AddLanguageAction {
    type: typeof ADD_LANGUAGE,
    payload: {
        name: string;
        code: string;
    }
}

export type LanguageActionTypes = ChangeLanguageAction | AddLanguageAction;

const changeLanguageActionCreator = (
    languageCode: "zh" | "en"
): ChangeLanguageAction => {
    return {
        type: CHANGE_LANGUAGE,
        payload: languageCode
    };
};

const addLanguageActionCreator = (
	name: string,
    code: string
): AddLanguageAction => {
    return {
        type: ADD_LANGUAGE,
        payload: { name, code }
    };
};
```

### 配置reducer

```tsx
interface LanguageState {
    language: "en" | "zh";
    languageList: {
        name: string;
        code: string;
    }[];
}

const defaultState: LanguageState = {
    language: "zh",
    languageList: [
        { name: "中文", code: "zh" },
        { name: "English", code: "en" }
    ]
};

export default (state = defaultState, action: LanguageActionTypes) => {
    switch (action.type) {
        case CHANGE_LANGUAGE:
            i18n.changeLanguage(action.payload); // 不推荐这样做，因为有副作用
            return { ...state, language: action.payload };
        case ADD_LANGUAGE:
            return {
                ...state,
                languageList: [...state.languageList, action.payload]
       }
       default:
            return state;
    }
}
```

### 配置store

```tsx
const store = createStore(reducer);
```

### 使用redux

```tsx
const action = addLanguageActionCreator("新语言", "new_lang");
store.dispatch(action);

const action = changeLanguageActionCreator(code);
store.dispatch(action);
```

