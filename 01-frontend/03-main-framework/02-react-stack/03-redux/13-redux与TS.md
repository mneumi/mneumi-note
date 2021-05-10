## react-redux

```shell
npm install @types/react-redux --save-dev
# OR
yarn add @types/react-redux -D
```



```tsx
import languageReducer from "./language/languageReducer";

const store = createStore(language);

type RootState = ReturnType<typeof store.getState>;

export default store;
```

```tsx
const mapStateToProps = (state: RootState) => {
    return {
        language: state.language,
        languageList: state.languageList
    }
}

const mapDispatchToProps = (dispatch: Dispatch) => {
    return {
        changeLanguage: (code: "zh" | "en") => {
            const action = changeLanguageCreator(code);
            dispatch(action);
        },
        addLanguage: (name: string, code: string) => {
            const action = addLanguageActionCreator(name, code);
            dispatch(action);
        }
    }
}
```

```tsx
type PropsType = RouteComponentProps & WithTranslation & ReturnType<typeof mapStateToProps> & ReturnType<typeof mapDispatchToProps>;
```



## hooks

### useSelector

```tsx
// 写法1
const language = useSelector((state: RootState) => state.language);
```

```tsx
// 写法2
import {
    useSelector as useReduxSelector,
    TypedUseSelectorHook
} from "react-redux";

import { RootState } from "./store";

export const useSelector: TypedUseSelectorHook<RootState> = useReduxSelector;
```

```tsx
const language = useSelector(state => state.language);
```

### useDispatch

```tsx
import { Dispatch } from "redux";

const dispatch = useDispatch<Dispatch<LanguageActionTypes>>();
```



## 通用流程1

```tsx
interface State {
    loading: boolean;
    error: string | null;
    productList: any[];
}

class HomePageComponent extends React.Component<WithTranlation, State> {
	constructor(props) {
        super(props);
        this.state = {
            loading: false,
            error: null,
            productList: []
        }
    }
    
    async componentDidMount() {
        try {
            const { data } = await axios.get("url");
            this.setState({
                loading: false,
                error: null,
                productList: data
            })
        } catch (error) {
            this.setState({
                error: error.message,
                loading: false,
                productList: null
            })
        }
    }
}
```



## redux中间件

```tsx
import { Middleware } from "redux";

export const actionLog: Middleware = (store) => (next) => (action) => {
    console.log("state 当前: ", store.getState());
   	console.log("fire action: ", action);
    next(action);
    console.log("state 更新: ", store.getState());
}
```





## redux-thunk

```tsx
export const giveMeDataActionCreator = (): ThunkAction<
	void,
    RootState,
    unknown,
    RecommendProductAction
> => async (dispatch, getState) => {
    dispatch(fetchRecommendProductStartActionCreator());
    try {
        const { data } = await axios.get("Url");
        dispatch(fetchRecommendProductSuccessActionCreator());
    } catch (error) {
        dispatch(fetchRecommendProductFailActionCreator(error.message));
    }
}
```

