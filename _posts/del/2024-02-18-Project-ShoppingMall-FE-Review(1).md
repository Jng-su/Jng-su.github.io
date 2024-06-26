---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE-REVIEW(1)
author: JEONGSUJONG
date: 2024-02-18 01:00:00 +0800
toc: true
pin: false
published: false
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## Router

앱 어디에서나 React Router를 사용할 수 있도록 하기 위해 `index.js` 파일에 `React-Rtouer-DOM` 라이브러리 내에 있는 `BrowserRouter` 를 사용한다.

### 🧷 BrowserRouter

```jsx
ReactDOM.render (
    <React.StrictMode>
        <App />
    </React.StrictMode>
    document.getElementById('root')
);
```

<br>

```jsx
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render (
    <BrowserRouter>
        <App />
    </BrowserRouter>
    document.getElementById('root')
);
```

- `<BrowserRouter>` 는 앱의 루트 노드에 위치시켜야한다.

<br>

### 🧷 중첩 라우팅

```jsx
<BrowserRouter>
    <Routes>
        <Route path="/" element={ <App /> }>
            <Route index element={ <Home /> }>
            <Route path="teams" element={ <Teams /> }>
                <Route index element={ <LeagueStanding /> }>
                <Route path=":teamId" element={ <Team /> }>
                <Route path="new" element={ <NewTeamForm /> }>
            </Route>
        </Route>
    </Routes>
</BrowserRouter>
```

- `<Routes>` : Route의 집합을 정의하는 라우터 컴포넌트이다.
- `<Route>` : 특정 경로에 대한 라우팅을 정의한다. (`path` 는 경로를 지정하고 `element` 는 해당 경로에 렌더링될 React 요소를 지정한다.)
- `index` 는 상위 경로에 해당하는 경로에 자동으로 렌더링 된다.

<br>

1. 루트 경로 `/` 경로에 대한 기본 페이지 `<Home />` 컴포넌트가 렌더링 된다.
2. `/teams` 경로로 `<Teams />` 컴포넌트가 렌더링된다.
3. `/teams` 경로에 대한 기본페이지 `<LeagueStanding />` 컴포넌트가 렌더링 된다.
4. `teamId` 경로는 동적으로 변하는 경로 파라미터이다. `new` 경로는 `<NewTeamForm />` 컴포넌트가 렌더링 된다.

<br>

> ## Outlet

- 중첩된 라우팅 구조에서 자식 경로의 컴포넌트를 렌더링할 때 사용된다.
- 자식 경로 요소를 렌더링하려면 부모 경로 요소에 `<Outlet />`을 사용해야한다.
- 하위 경로가 렌더링 될 때 중첩된 UI가 표시될 수 있다. (Ex, Header, Footer)

```jsx
function Layout() {
  return (
    <div>
      <h1>Welcome to the app</h1>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/teams">Teams</Link>
      </nav>
      <div className="content">
        <Outlet />
      </div>
    </div>
  );
}
```

<br>

> ## 👀 Router VS Outlet

```jsx
function Layout() {
  return (
    <div>
      <Navbar />
      <main>
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<LadingPage />} />

        {/* 로그인한 유저만 갈 수 있는 경로 */}
        <Route element={<ProtectedRoutes isAuth={isAuth} />}>
          <Route path="/protected" element={<ProtectedPage />} />
          <Route path="/product/upload" element={<UploadProductPage />} />
          <Route path="/product/:productId" element={<DetailProductPage />} />
          <Route path="/user/cart" element={<CartPage />} />
          <Route path="/history" element={<HistoryPage />} />
        </Route>

        {/* 로그인한 유저는 갈 수 없는 경로 */}
        <Route element={<NotAuthRoutes isAuth={isAuth} />}>
          <Route path="/login" element={<LoginPage />} />
          <Route path="/register" element={<RegisterPage />} />
        </Route>
      </Route>
    </Routes>
  );
}
```

- 위 코드는 현재 프로젝트의 Router 구조와 Outlet 사용법을 보여준다.
- `LayOut` 함수는 `main` 요소에 `Outlet`를 적용시켜 자식 경로에 요소들을 렌더링해준다.
- `App` 함수는 Router의 적용 예시이다
  1. 기본 렌딩 페이지는 앱에 접속 시 보여지는 화면이다. `/` 경로에 해당한다.
  2. 크게 로그인 유무로 페이지를 구분하였다. `isAuth` 가 `true` 이면 로그인한 유저가 갈 수 있는 페이지를 보여지게 된다. 반면 `false`이면 반대이다. (`ProtectedRoutes`, `NotAuthRoutes`)
  3. 나머지는 앞서 설명한 내용과 유사하니 PASS ... 🙄

<br>

> ## Redux

자바스크립트 애플리케이션을 위한 상태 관리 라이브러리이다. state안에서 데이터를 교환하고 state변경 시 컴포넌트가 리렌더링되게 한다.

<br>

Todo 앱으로 예를 들면, 상태 관리가 없는 앱같은 경우 각각의 할 일 항목은 컴포넌트의 로컬로 상태를 관리한다. 즉, 사용자가 할 일을 추가하거나 완료할 때마다 각 컴포넌트의 상태를 업데이트해야한다. 반면에 Redux를 사용하면 모든 할 일 항목을 중앙 상태 스토어에 저장가능하다. 각 컴포넌트는 필요한 상태를 스터오에서 직접 가져와 사용할 수 있으므로 상태관리가 쉬워지고 할 일을 추가할 때마다 액션을 디스패치하여 상태를 업데이트 할 수 있다.

<br>

### 🧷 Props VS State

- `Props`
  - A 컴포넌트에서 B 컴포넌트에 데이터를 전송할 때 사용한다.
  - 부모 컴포넌트에서 자녀 컴포넌트로 전송된다.
  - 해당 값을 변경하려면 자식 관전에서 Props를 변경할 수 있으며 부모는 내부 상태를 변경해야한다.

```jsx
<ChatMessages
    message={message}
    currentMember={member}
>
```

<br>

- `State`
  - `Props` 의 부모에서 자식으로 데이터를 보내는게 아닌 해당 Component 안에서 데이터를 전달할 때 사용된다.
  - State를 변경하면 리렌더링된다.

```jsx
state = {
    message = '',
    attachedFile: undefined,
    openMenu: False,
};
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/3e22950e-0f22-4e5d-8a50-4ae17459719a){: width=100% height=100%} -->

<br>

#### 📌 Action

- 간단한 Javascript 객체이다. 우리가 수행하는 작업의 유형을 지정하는 `type` 속성이 있다.

```javascript
{ type: 'LIKE_ARTICLE' , articleId: 42 }
{ type: 'FETCH_USER_SUCCESS' , response: { id:3 , name:'Mary' } }
{ type: 'ADD_TODO' , text: 'Read The Redux docs.' }
```

<br>

#### 📌 Reducer

- 앱의 상태 변경 사항을 결정하고 업데이트된 상태를 반환하는 함수이다. store 내부의 상태를 업데이트한다.

```javascript
{ previousState, action } => nextState
```

- 이전 State와 action object 를 받은 후에 nextState를 return 한다.

<br>

#### 📌 Redux Store

- Store 내부에는 State가 저장되어 있으며 action을 통해서만 State를 변경시킬 수 있다. 그리고 React Component에 전달해준다.

<br>

<!-- ![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/1f995892-6060-43c0-9ea1-08cb38de30b4) -->

<br>

#### 📌 Redux 실습으로 이해하기

<details>
<summary>🤖 미들웨어 없이 Redux Counter 앱 만들기</summary>
<div markdown="1">

<br>

- 리액트 앱 설치
  `npx create-react-app (my-app) --template typescript`

- 리덕스 라이브러리 설치
  `npm install redux --save`

<br>

- `Reducer` 만들기

```tsx
const counter = (state = 0, action: { type: string }) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
};

export default counter;
```

<br>

- `Store` 만들기

```tsx
import { createStore } from "redux";
import counter from "./reducers";

const store = createStore(counter);
```

- `createStore()` : 앱의 전체 상태 트리를 보유하는 Redux 저장소를 만든다. 앱에서는 하나의 스토어만 있어야한다.

<br>

```tsx
type Props = {
  value: number;
  onIncrement: () => void;
  onDecrement: () => void;
};

function App({ value, onIncrement, onDecrement }: Props) {
  return (
    <div className="App">
      Cliked: {value}
      <button onClick={onIncrement}>+</button>
      <button onClick={onDecrement}>-</button>
    </div>
  );
}

export default App;
```

<br>

```tsx
const store = createStore(counter);
const render = () =>
  root.render(
    <React.StrictMode>
      <App
        value={store.getState()}
        onIncrement={() => store.dispatch({ type: "INCREMENT" })}
        onDecrement={() => store.dispatch({ type: "DECREMENT" })}
      />
    </React.StrictMode>
  );

render();
store.subscribe(render);
```

- `getState()` : 앱의 현재 상태 트리를 반환한다. 스토어의 리듀서가 반환한 마지막 값과 같다.
- `dispatch()` : action을 보내서 상태를 업데이트한다.
- `subscribe()` : 스토어의 상태가 변경될 때 지정된 콜백 함수가 호출되게 한다. UI에 보여줌

</div>
</details>

<details>
<summary>🤖 ToDo 기능 추가하기</summary>
<div markdown="1">

<br>

- CombineReducers
  - 현재까지 Counter 리듀서만 있는데 하나를 더 추가하려면 Root 리듀서를 만들어서 그 아래 counter와 todos 라는 Sub 리듀서를 추가해야한다.
  - Root 리듀서를 만들 때 사용하는 것이 `combineReducers` 이다

```
└─src
    ├─reducers
    │   ├─counter.tsx
    │   ├─todos.tsx
    │   ├─index.tsx     // ROOT REDUCERS
```

- Root Reducers
### 🧷 Redux Middleware

- Redux 미들웨어는 Action을 dispatch 전달하고 리듀서에 도달하는 순간 사이에 사전에 지정된 작업을 실행할 수 있게 해주는 중간자이다.

- Locking , 충돌 보고 , 비동기 API 통신 , 라우팅 등을 위해 Redux Middleware을 사용한다.

<!-- ![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/12f15958-e789-4341-9ffb-8b9f40e2169b) -->

<br>

```tsx
const loggerMiddleware = (store: any) => (next: any) => (action: any) => {
  console.log("store:", store);
  console.log("action:", action);
  next(action);
}

const middleware = applyMiddleware(loggerMiddleware);
const store = createStore(rootReducers, middleware);
```
```tsx
import { combineReducers } from "redux";
import counter from "./counter";
import todos from "./todos";

const rootReducers = combineReducers({
  counter,
  todos
});

export default rootReducers;
```

- 루트 리듀서를 만들기 위해 각각의 서브 리듀서를 만들고 이를 `combineReducers` 함수에 전달하여 결합시킨다. 
- 이렇게 함으로써 Redux 스토어는 단일 리듀서만을 사용하는 것 처럼 작동하지만 서브 리듀서가 각자의 영역을 처리한다.

<br>

- todos Reducer 만들기

```tsx
enum ActionType {
  ADD_TODO = "ADD_TODO",
  DELETE_TODO = "DELETE_TODO"
}

interface Action {
  type: ActionType;
  text: string;
}

const todos = (state = [], action: Action) => {
  switch (action.type) {
    case "ADD_TODO":
      return [...state, action.text];
    default:
      return state;
  }
};

export default todos;
```

<br>

```tsx
import rootReducers from './reducers';

const store = createStore(rootReducers);
store.dispatch({
  type: "ADD_TODO",
  text: "HELLO REDUX"
})
console.log('store.getState', store.getState());
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/dece5816-21c5-4177-a860-357f31cec7cc){: width=100% height=100% .normal} -->

- 결과를 보기 위해 `dispatch` 메서드를 사용하여 특정 액션을 발생시켰다.
- `getState` 메서드를 사용하여 스토어의 현재 상태를 확인할 수 있다.

<br>

</div>
</details>

<br>

### 🧷 Provider

- `<Provider>` 구성 요소는 Redux Store 저장소에 엑세스해야 하는 모든 중첩 구성 요소에서 Redux Store 저장소를 사용할 수 있도록 한다. (Store에 접근가능하게 도와준다.)
- render 할 때 `Provider` 로 감싸주면 Store에 접근가능하다. 최상위에 위치시킨다.

`npm install react-redux --save`

<br>

```tsx
const render = () =>
  root.render(
    <React.StrictMode>
      <Provider store={store}>
        <App
          value={store.getState()}
          onIncrement={() => store.dispatch({ type: "INCREMENT" })}
          onDecrement={() => store.dispatch({ type: "DECREMENT" })}
        />
      </Provider>
    </React.StrictMode>
  );
```

<br>

#### 📌 Provider로 Store 접근하기

- `useSelector`
    - 스토어의 값을 가져온다.

```jsx
const counter = useSelector((state) => state.counter)
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/46af7bbb-f497-430b-8b91-5849a132f076){: width=100% height=100% .normal} -->

- Type 에러가 발생한다.

<br>

- 해결방법 : RootReducers

```tsx
export type RootState = ReturnType<typeof rootReducers>;
```
```tsx
const counter = useSelector((state: RootState) => state.counter);
```

<br>

- `useDispatch`
    - 스토어에 있는 `dispatch` 함수에 접근한다.

```tsx
  const dispatch = useDispatch();
  const counter = useSelector((state: RootState) => state.counter);
  const todos: string[] = useSelector((state: RootState) => state.todos);

  const [todoValue, setTodoValue] = useState("");
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setTodoValue(e.target.value);
  }

  const addTodo = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    dispatch({ type: "ADD_TODO", text: todoValue })
    setTodoValue("");
  }
```

<br>

### 🧷 Redux Middleware

- Redux 미들웨어는 Action을 dispatch 전달하고 리듀서에 도달하는 순간 사이에 사전에 지정된 작업을 실행할 수 있게 해주는 중간자이다.

- 로깅 , 충돌 보고 , 비동기 API 통신 , 라우팅 등을 위해 Redux Middleware을 사용한다.

<!-- ![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/12f15958-e789-4341-9ffb-8b9f40e2169b) -->

<br>

```tsx
const loggerMiddleware = (store: any) => (next: any) => (action: any) => {
  console.log("store:", store);
  console.log("action:", action);
  next(action);
}

const middleware = applyMiddleware(loggerMiddleware);
const store = createStore(rootReducers, middleware);
```

- `store` : Redux 스토어의 현재 상태를 보여준다.
- `next` : 다음 미들웨어로 전달하는 함수 (없다면 리듀서에 액션을 전달)
- `action` : 현재 처리 중인 액션
- `applyMiddleware` : 미들웨어를 Redux 스토어에 적용하는 함수

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/25ca5619-4409-4b26-946f-d4f417840b25){: width=100% height=100% .normal} -->