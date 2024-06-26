---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE (4)
author: JEONGSUJONG
date: 2024-02-01 00:00:00 +0800
toc: true
pin: false
published: false
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## Register Page with Tailwind

- tailwind css 처음 사용해서 적응하느라 힘들었다...😥 그래도 작성하고 VSCode에 작성한 css속성 에 hover하면 무슨 기능인지 나온다.

<br>

```jsx
const RegisterPage = () => {
  return (
    <section className="flex flex-col justify-center mt-20 max-w-[400px] m-auto">
      <div className="p-6 bg-white rounded-md shadow-md">
        <h1 className="text-3xl font-semibold text-center">회원가입</h1>
        <form className="mt-6">
          <div className="mb-2">
            <label
              htmlFor="email"
              className="text-sm font-semibold text-center"
            >
              Email
            </label>
            <input
              type="email"
              id="email"
              className="w-full px-4 py-2 mt-2 bg-white border rounded-md"
            />
          </div>
          ...
          <div className="mt-6">
            <button
              type="submit"
              className="w-full bg-black text-white px-4 py-2 rounded-md hover:bg-gray-700 duration-200"
            >
              회원가입
            </button>
            <p className="mt-8 text-xs font-light text-center text-gray-700">
              이미 가입된 회원이신가요?
              <a href="/login" className="font-medium hover:underline">
                로그인
              </a>
            </p>
          </div>
        </form>
      </div>
    </section>
  );
};
```

- `flex-col` : 아이템을 세로로 배치하는 Flexbox의 속성, 요소들을 세로(column)로 쌓는다
- `justify-center` : 부모 요소 내에서 자식 요소들을 수평 방향으로 가운데 정렬하는 Flexbox의 속성
- `text-center` : 텍스트를 수평 방향으로 가운데 정렬하는 CSS 속성, 텍스트를 가운데로 정렬
- `px` / `py` : `px`는 수평(padding-x) 방향의 안쪽 여백 지정하고 `py`는 수직(padding-y) 방향의 안쪽 여백 지정

<br>

### 🧷 React-Hook-Form

- 유효성 검사를 위한 React-Hook 사용

[https://react-hook-form.com/get-started](https://react-hook-form.com/get-started)

- `npm install react-hook-form`

<br>

- Register/index.jsx

```jsx
const {
  register,
  handleSubmit,
  formState: { errors },
  reset
} = useForm({ mode: "onChange" });
```

- `useForm` hook (register, handleSubmit, ,formState, reset 모두 useForm 훅에 있는 함수이다.)
  - `useForm` 은 폼을 초기화하는 `react-hook-form` 에서 제공된다.
  - `register` 함수는 입력 요소를 폼에 등록하고 유효성 검사를 가능하게 한다.

<br>

```jsx
const onSubmit = ({ email, password, name }) => {
  reset();
};
```

- `onSubmit` : 폼이 제출될 때 실행되는 함수로써 간단히 reset 함수를 호출하여 입력값을 초기화한다.

<br>

```jsx
  const userEmail = {
    required: "이메일을 입력해주세요.",
  };
  const userName = {
    required: "이름을 입력해주세요.",
  };
  const userPassword = {
    required: "비밀번호를 입력해주세요.",
    minLength: {
      value: 6,
      message: "최소 6자입니다.",
    },
  };
```

- 유효성 검사 규칙을 설정할 수 있다.

<br>

```jsx
<input
  type="email"
  id="email"
  className="w-full px-4 py-2 mt-2 bg-white border rounded-md"
  {...register("email", userEmail)}
/>
```

- 앞서 등록한 유효성 검사 함수인 `register` 로 입력 요소를 폼에 등록하고 해당 필드에 대한 유효성 검사를 한다.

<br>

```jsx
{errors?.email && (
  <div>
    <span className="text-red-500">{errors.email.message}</span>
  </div>
)}
```

- `errors` 객체를 통해 유효성 검사에서 발생한 에러를 확인하고 에러 메시지를 보낸다.

<br>

> ## Axios Instance

- Axios : 브라우저, Node.js 를 위한 Promise API 를 활용하는 HTTP 비동기 통신 라이브러리.
    - 쉽게 말해 BE와 FE가 통신을 쉽게하기 위해 Ajax와 더불어 사용한다.

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/7ff9b0a3-3e77-440c-adc5-4609e1786464){: width=100% height=100% .normal} -->

<br>

### 🧷 Axios

- `npm install axios --save`

- Axios를 사용할 때 중복된 설정을 피하기 위해 인스턴스를 생성하는 것이 일반적
    - 여러 요청에서 공통으로 사용할 설정 (baseURL)을 인스턴스에 정의
    - `localhost:4000/login?name="sujong"`
    - `localhost:4000/register?name="sujong"`

<br>

- utils/axios.js

```jsx
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: import.meta.env.PROD ? "" : "http://localhost:5000",
});

export default axiosInstance;
```

- `axios.create` : Axios 인스턴스를 생성한다.
- `import.meta.env.PROD`
    - `true` 인 경우는 배포 후 나오는 URL 설정
    - `false` 인 경우는 개발 환경 (서버의 로컬환경 URL 설정)

<br>

- axiosInstance 적용 예시

```javascript
import axiosInstance from "./utils/axios.js";

axiosInstance.get("/login", { params: { name: "sujong" } })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

- `/login` 엔드포인트로 `GET` 요청을 보내고 있으며, 필요에 따라 다양한 HTTP 메서드에 대한 요청을 설정할 수 있다.

<br>

> ## Register Functionality

- store/thunkFunction.js

```jsx
import { createAsyncThunk } from "@reduxjs/toolkit";
import axiosInstance from "../utils/axios";

export const registerUser = createAsyncThunk(
    "user/registerUser",
    async (body, thunkAPI) => {
        try {
            const response = await axiosInstance.post(
                `/users/register`,
                body
            )
            return response.data;
        } catch (error) {
            console.log(error);
            return thunkAPI.rejectWithValue(error.response.data || error.message);
        }
    }
);
```

- `registerUser` : 비동기 Thunk 함수를 생성한다.
    - `Thunk` 함수는 Redux Toolkit 에서 제공하는 `createAsyncThunk`를 사용하여 비동기 작업을 처리한다.
    - `Thunk` 함수는 사용자 등록 API에 대한 요청을 보내고 성공시 응답 데이터를 반환한다. 실패할 경우 `rejectWithValue` 라는 Redux Toolkit이 제공하는 에러 처리를 하게된다.

<br>

- store/userSlice.js

```jsx
const userSlice = createSlice({
    name: 'user',
    initialState,
    reducers: {},
    extraReducers: (builder) => { 
        builder
          .addCase(registerUser.pending, (state) => {
            state.isLoading = true;
          })
          .addCase(registerUser.fulfilled, (state) => {
            state.isLoading = false;
          })
          .addCase(registerUser.rejected, (state, action) => {
            state.isLoading = false;
            state.error = action.payload;
          });
    }
})
```

- `userSlice` 라는 Redux slice를 생성한다. slice는 Redux 상태와 Reducer를 관리하는데 사용된다.
- `extraReducer` 는 앞서 생성한 Thunk 함수의 상태 변화에 따라 Redux 상태를 업데이트한다.
    - 등록요청이 보내질 때, 성공, 실패했을 경우 상태 변화를 정의

<br>

- Register/index.jsx
```jsx
  const dispatch = useDispatch();

  const onSubmit = ({ email, password, name }) => {
    const body = {
      email,
      password,
      name,
      image: `https://via.placeholder.com/600x400?text=no+user+image`,
    };
    
    dispatch(registerUser(body));

    reset();
  };
```

- `useDispatch` hook을 사용하여 Redux의 `dispatch` 함수를 가져온다.
- Form 제출 (`onSubmit`) 시 입력된 사용자 정보를 포함한 객체를 생성하고 해당 객체를 `dispatch` 하여 `registerUser` Thunk 함수에 전달.
    - Form 제출 후 Reset

<br>

> ## React Toastify

[https://fkhadra.github.io/react-toastify/introduction/](https://fkhadra.github.io/react-toastify/introduction/)

- `npm install react-toastify`

```jsx
import { ToastContainer } from 'react-toastify'
import 'react-toastify/dist/ReactToastify.css'
```

<br>

- App.jsx

```jsx
function Layout() {
  return (
    <div className="flex flex-col h-screen justify-between">
      <ToastContainer
        position="bottom-right"
        autoClose={1500}
        theme="light"
        transition:Slide
      />
      <Navbar />
      <main className="mb-auto w-10/12 max-w-4xl mx-auto">
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}
```

- 위 사이트를 참고하면 쉽게 구현가능하다.

<br>

- store/userSlice.js

```javascript
const userSlice = createSlice({
    name: 'user',
    initialState,
    reducers: {},
    extraReducers: (builder) => { 
        builder
          .addCase(registerUser.pending, (state) => {
            state.isLoading = true;
          })
          .addCase(registerUser.fulfilled, (state) => {
              state.isLoading = false;
              toast.info('회원가입을 성공했습니다.')
          })
          .addCase(registerUser.rejected, (state, action) => {
            state.isLoading = false;
              state.error = action.payload;
              toast.info(state.payload);
          });
    }
})
```

<!-- ![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/28294404-55ca-4604-b40f-b4bd2bdc0cdf){: width=100% height=100% .normal} -->

- 아직 BE 라우터가 설정이 되지 않아 메시지가 이상하다. 😅