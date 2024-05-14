---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE (5)
author: JEONGSUJONG
date: 2024-02-07 01:00:00 +0800
toc: true
pin: false
published: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## User 인증 여부 체크

- 로그인 한 유저가 올바른 토큰 (유효기간)을 가지고 있는 지 체크
- App.jsx : 유저가 로그인 상태인지 확인, 로그인 상태라면 서버에 유저의 인증 여부를 확인하는 요청을 보낸다.

```jsx
const dispatch = useDispatch();
const isAuth = useSelector((state) => state.user?.isAuth);
const { pathname } = useLocation();

useEffect(() => {
  if (isAuth) {
    dispatch(authUser());
  }
}, [isAuth, pathname, dispatch]);
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/72e864dd-11be-460e-90e7-df732f59b71f){: width=100% height=100% .normal}

- Redux의 `useSelector` 훅을 사용하여 Redux store에서 유저의 인증 상태인 `isAuth` 를 가져온다.
- `useDispatch` 훅을 사용하여 Redux store에 action 을 dispatch 한다.
- `useEffect` 는 컴포넌트가 마운트될 경우 `isAuth` 와 `pathname`이 변경될 때마다 특정 작업을 수행하도록 설정

<br>

- thunkFunction.js : 서버에 유저의 인증 여부를 확인하는 요청을 보냄. (비동기)

```javascript
export const authUser = createAsyncThunk(
  "user/authUser",
  async (_, thunkAPI) => {
    try {
      const response = await axiosInstance.get(`/users/auth`);
      return response.data;
    } catch (error) {
      console.log(error);
      return thunkAPI.rejectWithValue(error.response.data || error.message);
    }
  }
);
```

- `createAsyncThunk` 함수를 이용하여 비동기 작업을 처리한다.
- 이 함수는 서버로부터 유저의 인증 여부 확인하는 API를 호출하고 그 결과를 반환한다.

<br>

- axios.js : 모든 서버 요청에 토큰을 함께 보내준다.

```javascript
axiosInstance.interceptors.request.use(
  function (config) {
    config.headers.Authorization =
      "Bearer " + localStorage.getItem("accessToken");
    return config;
  },
  function (error) {
    return Promise.reject(error);
  }
);
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/386d91c4-9e12-4223-919c-abeb0274ba9f){: width=100% height=100% .normal}

- 모든 HTTP 요청에 대해 Authorization 헤더에 유효한 토큰을 추가한다.
- 이를 통해 서버에 요청할 때마다 유저의 인증 상태를 확인할 수 있다.

<br>

- userSilce.js : `createSilce` 에서 액션인 `authUser` 에 대한 리듀서를 정의한다.

```javascript
// Auth
.addCase(authUser.pending, (state) => {
  state.isLoading = true;
})
.addCase(authUser.fulfilled, (state, action) => {
  state.isLoading = false;
  state.userData = action.payload;
  state.isAuth = true;
})
.addCase(authUser.rejected, (state, action) => {
  state.isLoading = false;
  state.error = action.payload;
  state.isAuth = false;
  localStorage.removeItem('aceessToken');
});
```

<br>

> ## NotAuthRoutes/ProtectedRoutes

- 로그인이 되어있을 경우(`isAuth==true`) 로그인/회원가입 접근 안되게 해야함.
- 반대로 로그인을 안했을 경우 특정 페이지에 접근 불가하게 해야함
- `ProtectedRoutes` : 로그인 안한 유저가 해야하는 페이지로 이동
- `NotAuthRoutes` : 로그인을 한 유저가 회원가입 및 로그인으로 들어가려면 `'/'` 경로로 redirect 시켜준다.
  - `Outlet` : 부모 경로에서 하위 경로를 렌더링할 때 사용된다. 예를 들어 부모 경로에서 하위 경로의 컴포넌트를 보여주기 위해 사용된다,
  - `Navigate` : 특정 경로로 redirection 수행하는 컴포넌트이다. 사용자를 다른 경로로 이동시키고 싶을 때 사용. (`to` prop을 통해 이동하고자 하는 경로 설정)

<br>

- components/ProtectedRoutes.jsx

```jsx
import React from "react";
import { Navigate, Outlet } from "react-router-dom";

const ProtectedRoutes = ({ isAuth }) => {
  return isAuth ? <Outlet /> : <Navigate to={"/login"} />;
};

export default ProtectedRoutes;
```

- `isAuth` prop은 현재 사용자가 인증 여부를 판단하고 인증이 되지 않은 경우 로그인 페이지(`/login`)로 redirection 한다.

<br>

- components/NotAuthRoutes.jsx

```jsx
import React from "react";
import { Navigate, Outlet } from "react-router-dom";

const NotAuthRoutes = ({ isAuth }) => {
  return isAuth ? <Navigate to={"/"} /> : <Outlet />;
};

export default NotAuthRoutes;
```

<br>

- App.jsx

```jsx
return (
  <Routes>
    <Route path="/" element={<Layout />}>
      <Route index element={<LadingPage />} />

      {/* 로그인한 유저만 갈 수 있는 경로 */}
      <Route element={<ProtectedRoutes isAuth={isAuth} />}>
        <Route path="/protected" element={<ProtectedPage />} />
      </Route>

      {/* 로그인한 유저는 갈 수 없는 경로 */}
      <Route element={<NotAuthRoutes isAuth={isAuth} />}>
        <Route path="/login" element={<LoginPage />} />
        <Route path="/register" element={<RegisterPage />} />
      </Route>
    </Route>
  </Routes>
);
```

- `Routes` 컴포넌트 내에서 `Route` 컴포넌트를 사용하여 각 경로에 대한 라우팅을 설정한다. `element` prop을 사용하여 해당 경로에 렌더링할 컴포넌트를 지정한다.

```jsx
const isAuth = useSelector((state) => state.user?.isAuth);
```

- `useSelector` hook은 Redux store의 상태를 인자로 받아 특정 부분의 상태를 선택한다.
- `state.user?.isAuth` 는 Redux store 의 `user` 객체에서 `isAuth` 값을 선택한다. (`?.`는 옵셔널 체이닝(optional chaining) 연산자로 `user` 객체가 존재하지 않거나 `isAuth` 가 존재하지 않는 경우에도 안전하게 접근할 수 있도록 한다.)
- 즉, `isAuth` 는 Redux store 에서 가져온 사용자의 인증상태를 나타내는 값이다.

<br>

> ## Navbar

```jsx
import React, { useState } from "react";
import { Link } from "react-router-dom";
import NavItem from "./Sections/NavItem";

const Navbar = () => {
  // 메뉴 상태를 관리하는 useState 훅을 사용하여 메뉴의 열림/닫힘 여부를 저장
  const [menu, setMenu] = useState(false);

  // 메뉴를 열고 닫는 함수
  const handleMenu = () => {
    setMenu(!menu);
  };

  return (
    // 네비게이션 바를 감싸는 nav 요소
    <nav className="relative z-10 text-white bg-black">
      <div className="w-full">
        <div className="flex items-center justify-between mx-5 sm:mx-10 lg:mx-20">
          {/* 로고 섹션 */}
          <div className="flex items-center text-4xl h-16">
            {/* 홈으로 이동할 수 있는 로고 링크*/}
            <Link to="/">Logo</Link>
          </div>

          {/* 메뉴 버튼 섹션. 작은 화면 크기에서 표시 */}
          <div className="text-2xl sm:hidden">
            {/* 메뉴 버튼을 클릭하면 handleMenu 함수가 호출되어 메뉴 여닫이 가능 */}
            <button onClick={handleMenu}>{menu ? "-" : "+"}</button>
          </div>

          {/* 데스크톱 화면 크기에서 표시되는 네비게이션 메뉴 섹션. */}
          <div className="hidden sm:block">
            {/* NavItem 컴포넌트를 사용하여 네비게이션 메뉴를 표시. */}
            <NavItem />
          </div>
        </div>

        {/* 모바일 화면 크기에서 표시되는 네비게이션 메뉴 섹션. */}
        <div className="block sm:hidden">
          {/* 메뉴가 열려있을 때에만 NavItem 컴포넌트를 표시. */}
          {menu && <NavItem />}
        </div>
      </div>
    </nav>
  );
};

export default Navbar;
```

### 🧷 Navbar Item

```jsx
import React from "react";
import { useSelector } from "react-redux";
import { Link } from "react-router-dom";

const routes = [
  { to: "/login", name: "SIGN IN", auth: false },
  { to: "/register", name: "SIGN UP", auth: false },
  { to: "", name: "LOGOUT", auth: true }
];

const NavItem = ({ mobile }) => {
  const isAuth = useSelector((state) => state.user?.isAuth);
  const handleLogout = () => {};

  return (
    <ul
      className={`text-sm justify-center w-full gap-4 ${
        mobile ? "flex-col bg-white text-black h-full items-center" : ""
      }`}
    >
      {routes.map(({ to, name, auth }) => {
        if (isAuth !== auth) return null;
        if (name === "LOGOUT") {
          return (
            <li
              key={name}
              className="py-2 text-center border-b-2 cursor-pointer"
            >
              <Link onClick={handleLogout}>{name}</Link>
            </li>
          );
        } else {
          return (
            <li
              key={name}
              className="py-2 text-center border-b-2 cursor-pointer"
            >
              <Link to={to}>{name}</Link>
            </li>
          );
        }
      })}
    </ul>
  );
};

export default NavItem;
s;
```
