---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE (3)
author: JEONGSUJONG
date: 2024-01-31 00:00:00 +0800
toc: true
pin: false
published: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## Tailwind 적용하기

- App.jsx

```jsx
function Layout() {
  return (
    <div className="flex flex-col h-screen justify-between">
      <Navbar />
      <main className="mb-auto w-10/12 max-w-4xl mx-auto">
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}
```

- 부모 `div` 에서 `flex` 로 설정하여 자식 요소들이 `flex` 아이템이 되게 설정한다.
- `flex-col` : flex 방향을 세로로 설정한다. 자식 요소(`Navbar`, `main`, `Footer`) 역시 세로로 쌓이게 된다.
- `h-screen` : flex 컨테이너의 높이를 화면 높이의 100%로 설정한다. flex 컨테이너가 화면 전체 높이를 차지한다.
- `justyfy-between` : 주 축을 따라 콘텐츠를 정렬한다. (이 경우에는 세로) , `Navbar` 를 화면 상단, `Footer` 를 화면 하단에 위치시켜 그 사이에 공간을 만든다.
- `md-auto` : `main` 요소의 하단 마진을 자동으로 설정한다. 이로써 `Navbar` 와 `Footer` 사이에서 가능한한 많은 공간을 차지하며 `Footer` 를 화면 하단으로 밀어낸다.
- `w-10/12` : `main` 요소의 너비를 부모 컨테이너의 약 83%로 너비를 차지한다.
- `max-w-4xl` : `main` 요소의 최대 너비 (`max-w`) 를 사전 정의된 값으로 설정
- `max-auto` : 좌우 마진을 자동으로 설정한다.

<br>

> ## React Icons

[https://react-icons.github.io/react-icons/](https://react-icons.github.io/react-icons/)

`npm install react-icons`

- Footer/index.jsx

```jsx
import React from "react";
import { AiOutlineSmile } from "react-icons/ai";
const Footer = () => {
  return (
    <div className="flex h-20 text-lg items-center justify-center bg-gray-800 text-white">
      All rights reserved. <AiOutlineSmile />
    </div>
  );
};
export default Footer;
```

<br>

> ## Redux

`npm install @reduxjs/toolkit react-redux`

- store/userSlice.js

```javascript
const initialState = {
    userData: {
        id: '',
        email: '',
        name: '',
        role: 0,
        image: '',
    },
    isAuth: false,
    isLoading: false,
    error: ''
};
```

- `initalState` 는 슬라이스 초기 상태를 정의한다.
- 사용자 데이터, 인증 상태, 로딩 여부, 오류 메시지를 포함한다.

<br>

```javascript
const userSlice = createSlice({
    name: 'user',         // 슬라이스의 이름
    initialState,         // 초기 상태
    reducers: {},         // 리듀서 액션 생성자 함수들이 정의되는 곳
    extraReducers: (builder) => {}  // 비동기 액션에 대한 추가 리듀서
});

export default userSlice.reducer;
```

- `createSlice` 함수는 슬라이스 객체를 생성한다.
    - `reducers` : 리듀서 함수를 정의할 수 있다. 리듀서는 액션이 발생했을 경우 상태를 어떻게 변경할지를 정의한다.
    - `extraReducers` : Redux Toolkit에서 비동기 액션에 대한 추가적인 리듀서를 정의할 수 있는 부분이다.
- 이 슬라이스는 Redux 스토어에 통합되고, 액션 및 리듀서 함수를 추가하여 상태를 업데이트하고 관리할 수 있다.

<br>


### 🧷 Redux store 생성

- store/index.js

```javascript
import { configureStore } from "@reduxjs/toolkit";
import userReducer from "./userSlice";

export const store = configureStore({
    reducer: {
        user: userReducer
    }
})
```

- Redux 스토어는 `configureStore` 함수를 사용하여 생성한다.
- `reducer` 속성에는 애플리케이션에 사용할 리듀서들을 등록한다.
    - 여기서는 `userReducer` 를 `user` 슬라이스에 등록

<br>

#### 📌 Provider

- main.jsx 

```jsx 
import { Provider } from "@reduxjs/toolkit";
import { store } from "./store";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <Provider store={store}>
      <App />
    </Provider>
  </BrowserRouter>
);
```

- `Provider` 컴포넌트는 React 애플리케이션에 Redux 스토어를 제공한다.
- `store` prop에는 위에서 생성한 Redux 스토어가 전달된다.
- 이렇게 함으로써 애플리케이션 내의 모든 컴포넌트가 Redux 스토어의 상태에 접근하고 업데이트할 수 있게 된다.
- Redux 스토어에 정의된 상태와 리듀서를 사용하여 컴포넌트들이 상태를 공유하고 업데이트할 수 있다.

<br>

### 🧷 Redux Persist

- Redux store에 있는 State들은 페이지를 새로고침 하면 초기화되는 것을 볼 수 있다.
- 하지만, Redux Persist를 사용하면 페이지 새로고침 후에도 상태를 유지한다.

`npm i redux-persist`

<br>

#### 📌 Redux Toolkit

- Redux Toolkit 을 사용하여 Redux store을 설정하고 그 상태를 지속시키기 위해 redux-persist를 사용한다.

<br>

- store/index.js

```javascript
import { combineReducers, configureStore } from "@reduxjs/toolkit";
import userReducer from "./userSlice";
import storage from "redux-persist/lib/storage";
import {
    FLUSH, PAUSE, PERSIST, PURGE, REGISTER,
    REHYDRATE, persistReducer, persistStore
} from "redux-persist";

// rootReducer를 생성, 여기에서는 userReducer를 포함하도록 설정
const rootReducer = combineReducers({
    user: userReducer,
});

// redux-persist를 설정하기 위한 구성 옵션을 정의
const persistConfig = {
    key: "root",        // localStorage에 저장될 때 사용할 키 이름
    storage,            // redux-persist가 사용할 저장소 (이 경우엔 localStorage)
    // whiteList: [],   // 특정 리듀서만 localStorage에 저장하도록 허용하는 옵션
    // blackList: []    // 특정 리듀서를 제외하고 나머지는 저장하지 않도록 하는 옵션
};

// redux-persist를 사용하여 영속성이 있는 리듀서를 생성
const persistedReducer = persistReducer(persistConfig, rootReducer);

// configureStore를 사용하여 Redux store를 생성
export const store = configureStore({
    reducer: persistedReducer,   // 위에서 생성한 영속성이 있는 리듀서를 사용
    middleware: getDefaultMiddleware => getDefaultMiddleware({
        serializableCheck: {
            ignoredActions: [FLUSH, REHYDRATE, PAUSE, PERSIST, PURGE, REGISTER]
        }
    })
});  // 리덕스 툴킷에서 제공하는 기본 미들웨어를 사용하며, serializableCheck 옵션을 통해 특정 액션들을 무시

// Redux store의 상태를 지속시키기 위한 persistor를 생성
export const persistor = persistStore(store);
```

- `combineReducers` : 여러 개의 리듀서를 하나로 합치는 역할을 합니다. 여기에서는 `userReducer`를 포함한 `rootReducer`를 생성한다.
- `persistReducer` : redux-persist와 함께 사용되어 영속성이 있는 리듀서를 생성한다.
- `configureStore` : Redux Toolkit에서 제공하는 메소드로, 기본 설정과 함께 Redux store를 생성한다.

<br>

- `getDefaultMiddleware` : Redux Toolkit에서 제공하는 기본 미들웨어를 사용한다.serializableCheck를 통해 몇 가지 특정 액션들을 무시한다.
    - serialize : `object` 값을 `string` 값으로 변환 (`JSON.stringify`)
    - deserialize : `string` 값을 `object` 값으로 변환 (`JSON.parse`)
        - action에 직렬화(serialize)가 불가능한 값 (non-serializable value)을 전달되면 에러가 나온다..!
    - action이 디스패치하게 될 때 serialize 한 function이 들어가 있어서 에러가 나옴.
        - redux persist를 사용할 때 이러한 에러를 안보이게 하려면 `serializableCheck`를 `false` 하면 에러가 안나옴.

<br>

- `persistStore` : Redux store의 상태를 지속시키기 위한 `persistor`를 생성한다.

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/e702770c-dc83-4ec0-945e-6d146491d95c){: width=100% height=100% .normal}
- 참고 : [https://chromewebstore.google.com/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=ko](https://chromewebstore.google.com/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=ko)

<br>

- main.jsx

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { BrowserRouter } from 'react-router-dom'
import { Provider } from "react-redux";
import { persistor, store } from "./store";
import { PersistGate } from 'redux-persist/integration/react'

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <App />
      </PersistGate>
    </Provider>
  </BrowserRouter>
);
```