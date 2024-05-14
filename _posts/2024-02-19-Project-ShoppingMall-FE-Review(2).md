---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE-REVIEW(2)
author: JEONGSUJONG
date: 2024-02-19 01:00:00 +0800
toc: true
pin: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## Redux Thunk

- Redux Thunk 는 Redux 미들웨어 중 하나로 action 에서 비동기 작업을 처리하도록 도와준다.
- 이를 통해서 action 함수를 반환하여 비동기 작업을 수행하고 작업 완료 후 action을 dispatch할 수 있다.

<br>

- Thunk : 일부 지연된 작업을 수행하는 코드 조각

```javascript
// 즉시 계산
let x = 1 + 2;

// 지연 계산
let y = () => 1 + 2;
// y 는 나중에 호출 될 수 있다. y 는 thunk 이다!
```

- 비동기 사용하는 이유 : 여러 경우 중 서버에 요청을 보내서 데이터를 가져올 때 주로 비동기를 사용한다.
  - 사용자의 입력을 기다리거나 외부 리소스를 기다리는 동안에도 애플리케이션이 멈추지 않고 계속 실행될 수 있게 해준다.
  - 성능 향상, 사용자 경험 개선, 자원 효율성, 동시성, 외부 리소스와의 상호 작용

<br>

> ## 유효성 검사

- React-Hook

`npm install react-hook-form`

```jsx
function Register() {
  const {
    register,
    handleSubmit,
    formState : {error},
    reset
  } = useForm ({ mode: "onChange" })
};
```

- `userForm` : 폼을 초기화한다.
- `{ mode: "onChange" }` : 입력값이 변경될 때마다 유효성 검사를 실행하도록 설정한다.

<br>

- Form 이 제출될 대 실행하는 함수

```jsx
const dispatch = useDispatch();

const onSubmit = ({ email, password, name }) => {
  const body = {
    email,
    password,
  };

  dispatch(loginUser(body));
  reset(); // 입력값 초기화
}
```

<br>

- 유효성 검사 규칙 설정

```jsx
const userEmail = {
  required : "이메일을 입력해주세요."
};

const userName = {
  required : "이름을 입력해주세요."
};

const userPassword = {
  required : "비밀번호를 입력해주세요.",
  minLength : {
    value : 6,
    message : "최소 6자 이상 입력해주세요."
  },
}
```

<br>

```jsx
<input
  type="email"
  id="email"
  {...register("email", userEmail)}
/>
```

- Spread 문법 `{...}` 을 사용하면 `register("email", userEmail)` 이 반환한 객체의 속성들을 해당 JSX 요소에 전달할 수 있다. 이를 통해 입력 요소에 필요한 속성들을 간단하게 전달할 수 있다.
- `register` : 입력 요소를 폼에 등록하고, 해당 필드에 대한 유효성 검사 규칙을 설정한다.


<br>

> ## Axios

- Axios : 브라우저, Node.js 를 위한 Promise API 를 활용하는 HTTP 비동기 통신 라이브러리
- 즉, BackEnd 와 FrontEnd 가 통신을 쉽게하기 위해 Ajax와 더불어 사용된다.

`npm install axios`

### 🧷 Axios Instance 생성

```jsx
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: import.meta.env.PROD ? "" : "http://localhost:5000",
});

export default axiosInstance;
```

- `axios.create` : Axios 인스턴스를 생성한다.
- `import.meta.env.PROD` : "(배포 후 나오는 URL 경로 설정)" : "(개발 환경 서버의 URL 주소)"

<br>

#### 📌 Axios Instance 사용 예시

```jsx
import axiosInstance from "./utils/axios.js";

axiosInstance.get("/login", { params: { name: "sujong" } })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

<br>

> ## Register Functionality 

- thunkFunction.js

```jsx
import { createAsyncThunk } from "@reduxjs/toolkit";
import axiosInstance from "../utils/axios";

// registerUser라는 thunk 함수 생성
export const registerUser = createAsyncThunk(
    // 액션 타입 정의
    "user/registerUser",
    // 비동기 작업을 수행하는 함수
    async (body, thunkAPI) => {
        try {
            // 서버에 POST 요청 보내고 응답을 기다림
            const response = await axiosInstance.post(
                `/users/register`,
                body
            )
            // 성공적으로 응답 받으면 데이터 반환
            return response.data;
        } catch (error) {
            // 요청 실패 시 오류 처리
            console.log(error);
            // thunkAPI.rejectWithValue를 통해 오류 처리
            return thunkAPI.rejectWithValue(error.response.data || error.message);
        }
    }
);
```