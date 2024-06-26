---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE (2)
author: JEONGSUJONG
date: 2024-01-30 00:00:00 +0800
toc: true
pin: false
published: false
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## React Router DOM

- React Router DOM 을 사용하면 웹 앱에서 동적 라우팅을 구현할 수 있습니다. 라우팅이 실행 중인 웹 외부의 구성에서 처리되는 기존 라우팅 아키텍쳐와 달리 React Router DOM은 웹 및 플랫폼의 요구 사항에 따라 컴포넌트 기반 라이팅을 용이하게 한다.

<br>

### 🧷 Single Page Application

- React는 SPA 이기 때문에 하나의 index.html 탬플릿 파일을 가지고 있다. 하나의 템플릿에 자바스크립트를 이용해서 다른 컴포넌트를 이 index.html 템플릿에 넣으므로 페이지를 변경해주게 된다.
- 이 때 React Router DOM 라이브러리가 새 컴포넌트로 라우팅/탐색을 하고 렌더링 하는데 도움이 된다.

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/2f2e51c4-8daa-49d9-b6a4-f41d16b4d6b9) -->

<br>

### 🧷 React Router 설정하기

- 앱 어디서나 React Router를 사용할 수 있도록 하기 위해 src 폴더에서 index.js 파일에 React-Router-DOM에 있는 BrowserRouter를 가져와 루트 구성 요소를 래핑한다.

<br>

#### 📌 BrowserRouter로 루트 컴포넌트 감싸기

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

<br>

#### 📌 중첩 라우팅

- 복잡한 레이아웃 코드를 어지럽히지 않고 url 세그먼트에 연결시킬 수 있다.

```jsx
<BrowserRouter>
    <Routes>
        <Route path="/" element={ <App /> }>
        { /* App 컴포넌트에 Header Footer를 Layout */ }
            <Route index element={ <Home /> }>
            { /* "/" 경로의 Home 컴포넌트 */ }
            <Route path="teams" element={ <Teams /> }>
            { /* "/teams" 경로의 Teams 컴포넌트 */ }
                <Route index element={ <LeagueStanding /> }>
                { /* "/teams" 경로의 LeagueStainding 컴포넌트 */ }
                <Route path=":teamId" element={ <Team /> }>
                <Route path="new" element={ <NewTeamForm /> }>
            </Route>
        </Route>
    </Routes>
</BrowserRouter>
```

<br>

#### 📌 Outlet

- 자식 경로 요소를 렌더링하려면 부모 경로 요소에 `<Outlet />`을 사용해야 한다.
- 하위 경로가 렌더링될 때 중첩된 UI가 표시될 수 있다 (Header, Footer)

```jsx
function App() {
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
    )
}
```

<br>

### 🧷 여러 컴포넌트 생성 및 라우트 정의

```jsx
function App() {
    return (
        <div className="App">
            <Routes>
                <Route path="/" element={ <Home /> } />
                <Route path="/about" element={ <About /> } />
                <Route path="/contact" element={ <Contact /> } />
            </Routes>
        </div>
    )
}
```

- Routes : 앱에서 생성될 모든 개별 경로에 대한 컨테이너/상위 영역
    - Route로 생성된 자식 컴포넌트 중 매칭되는 첫번째 Route를 렌더링
- Route : 단일 경로를 만드는 데 사용
    - `path` : 원하는 컴포넌트의 url 지정
    - `element` : 경로에 맞게 렌더링 되어야 하는 컴포넌트 지정

<br>

#### 📌 Link

```jsx
import { Link } from "react-router-dom";

function Home() {
    return (
        <div>
            <h1>HomePage</h1>
            <Link to="about">Showing About Page</Link>
            <Link to="contact">Showing Contact Page</Link>
        </div>
    )
}

export default Home;
```

- Link 구성 요소는 HTML의 앵커 요소(`<a />`)와 유사
to 경로는 사용자를 데려가는 경로로 지정
- 앱 구성 요소에 나열된 경로 이름을 생성했기 때문에 링크를 클릭하면 경로를 살펴보고 해당 경로 이름으로 구성 요소를 렌더링

<br>

> ## React Router Dom 적용하기

`npm install react-router-dom`

- main.jsx

```jsx
ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

<br>

- App.jsx

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
        <Route path="/login" element={<LoginPage />} />
        <Route path="/register" element={<RegisterPage />} />
      </Route>
    </Routes>
  );
}
```