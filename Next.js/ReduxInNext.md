# Redux in Next.js

> [진행하던 프로젝트](https://github.com/ChoJinmok/ikehaeyeo)를 초기세팅하던 도중 Next.js와 Redux를 함께 세팅해주면서 알게된 것을 정리했습니다.

<br />

## 1. Next.js와 Redux

- Next.js 서버 하나에, 수많은 브라우저들이 접속하는 경우 Next.js 서버는 Pre-Rendering 한 HTML페이지를 "모든 브라우저에게 각각" 보내고 Redux의 전역상태 관리 코드가 포함된 JS도 "모든 브라우저에게 각각" 전달된다.
- 즉, Next.js는 하나의 서버에서 돌아가고 Redux는 각각의 브라우저에서 돌아간다.
- Next.js 서버에서 특정 브라우저의 Redux 상태를 사용하려면 다음과 같은 과정이 필요하다.
  - 해당 브라우저의 리덕스 store 상태만 가져와서 수정
  - 다시 해당 브라우저의 store와만 동기화  
    => [next-redux-wrapper](https://github.com/kirill-konshin/next-redux-wrapper) 라이브러리는 이 작업을 대신해준다.

<br />

## 2. next-redux-wrapper 작동 과정

1. Next.js 서버 상에서 특정 브라우저의 리덕스 상태값을 변경하는 등의 Action을 Dispatch
2. next-redux-wrapper가 내부적으로 HYDRATE 라는 액션을 발생시킨다.
3. 브라우저와 동일한 상태의 Store를 서버에서 새로 생성해서 수정하고 동기화한다.

> Next.js 서버에서 브라우저의 Redux 에 접근하지 않는다면, next-redux-wrapper를 사용할 필요가 없다. (서버에서 구동되는 getServerSideProps, getStaticProps 등에서 Redux를 사용하지 않는 경우)

<br />

## 3. Next.js + ReduxToolkit + TypeScript 설정

### 3.1. rootReducer 파일

```typescript
import { combineReducers, AnyAction } from "@reduxjs/toolkit";

import { HYDRATE } from "next-redux-wrapper";

import signUpSlice, { SignUpState } from "./signUpSlice";

export interface ReducerStates {
  signUp: SignUpState;
}

const reducer = (state: ReducerStates, action: AnyAction) => {
  // Next.js 서버 상에서 특정 브라우저의 리덕스 상태값을 변경 Action발생 시 HYDRATE 액션이 일어나서 아래 코드 실행
  if (action.type === HYDRATE) {
    return {
      ...state,
      ...action.payload,
    };
  }

  return combineReducers({
    signUp: signUpSlice,
  })(state, action);
};

export default reducer;
```

### 3.2. store 파일

```typescript
import {
  configureStore,
  Reducer,
  AnyAction,
  ThunkAction,
  Action,
} from "@reduxjs/toolkit";

import { createWrapper } from "next-redux-wrapper";

import rootReducer, { ReducerStates } from "./modules";

const makeStore = () =>
  configureStore({
    reducer: rootReducer as Reducer<ReducerStates, AnyAction>,
    devTools: process.env.NODE_ENV === "development",
  });

// next-redux-wrapper 라이브러리의 wrapper를 만들어준다.
const wrapper = createWrapper(makeStore, {
  debug: process.env.NODE_ENV === "development",
});

// type export
export type AppStore = ReturnType<typeof makeStore>; // store 타입
export type RootState = ReturnType<typeof rootReducer>; // RootState 타입
export type AppDispatch = AppStore["dispatch"]; // dispatch 타입
export type AppThunk<ReturnType = void> = ThunkAction<
  ReturnType,
  RootState,
  unknown,
  Action
>; // Thunk 를 위한 타입

export default wrapper;
```

### 3.3. \_app.tsx

```tsx
import { Provider } from "react-redux";

import type { AppProps } from "next/app";

import wrapper from "../store";

export default function App({ Component, ...rest }: AppProps) {
  // useWrappedStore 사용해서 store와 props 생성
  const { store, props } = wrapper.useWrappedStore(rest);

  return (
    <Provider store={store}>
      <Component {...props.pageProps} />
    </Provider>
  );
}
```

- 과거엔 `wrapper.withRedux`메서드를 사용해서 app 컴포넌트를 export 해줬었다.

  ```tsx
  import type { AppProps } from "next/app";

  import { wrapper } from "../store";

  function App({ Component, pageProps }: AppProps) {
    return <Component {...pageProps} />;
  }

  // wrapper가 Provider store = {store} 까지 알아서 등록해준다.
  export default wrapper.withRedux(App);
  ```

> [메뉴얼](https://github.com/kirill-konshin/next-redux-wrapper)에는 slice 파일의 extraReducers에서 HYDRATE 액션을 처리해줄 수도 있다.
> 또, redux-saga 사용시 주의 사항도 나와있다.
