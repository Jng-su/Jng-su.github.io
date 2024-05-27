---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (04)
author: JEONGSUJONG
date: 2024-01-22 00:00:00 +0800
toc: true
pin: false
published: true
categories: [lecture, ReactJS로 영화 웹 만들기]
tags: [react]
# image: https://github.com/JEONGSUJONG/Readme_main/assets/142254876/7dd6d929-f416-492a-b255-f17f99c5b5a7
---

<br>

> ## npx create-react-app

- `npx create-react-app (projectName)`
    - `projectName` 대신 `.` 를 쓰면 현재 폴더에 저장된다. (현재 폴더 이름이 프로젝트 이름이 된다.)

- 초기화 작업

```
├─ node_modules
├─ public
│   ├─ favicon.ico
│   ├─ index.html
│   ├─ manifest.json
│   ├─ robots.txt
├─ src
│   ├─ App.js
│   ├─ Button.js
│   ├─ index.js
│   ├─ Button.module.css
├─ .gitignore
├─ package-lock.json
├─ package.json
├─ README.md
```

- `public`
    - 웹 서버에서 직접 제공되는 정적 자산을 저장하는 데 사용.
    - index.html : React 애플리케이션의 주요 HTML 파일 (root Element 포함)

- `src`
    - 애플리케이션의 소스 코드의 집합소!
    - App.js : React 애플리케이션의 주요 컴포넌트 (앱의 구조와 동작을 정의)
    - index.js : React 애플리케이션의 진입점 (root 포함) div 에 root 컴포넌트를 렌더링하는 역할.

<br>

> ## Understading Folder Structure

- 전 게시글의 props를 React 환경(?) 테스트하고 폴더 구조 흐름을 이해한다.

<br>

-  `src/Button.js`

```jsx
function Button(props) {
    return <button>{props.label}</button>;
}
export default Button;
```

- `export default Button;` : App.js 파일에 보내주기 위함으로 사용된다.

<br>

- `src/App.js`

```jsx
import Button from "./Button";

function App() {
  return (
    <div>
      <h1>Hello</h1>
      <Button label={"Click Me"} />
    </div>
  );
}
export default App;
```

- `import Button from "./Button` : export 한 Button 태그를 가져온다.
- `export default App; : index.js` 파일에 보내주기 위함으로 사용된다.

<br>

- `src/index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- `import React from 'react';` , `import ReactDOM from 'react-dom/client';` : `react` 는 React 앱을 만들 때 필요한 핵심 라이브러리, `react-dom` 은 React를 사용하여 웹 페이지에 컴포넌트를 렌더링하는 데 도움을 주는 라이브러리
- App 컴포넌트 가져오기ReactDOM.createRoot(document.getElementById('root')) : HTML 에서 id 가 root 인 Element 를 찾아서 React 앱이 이 곳에 렌더링 하도록 해준다.
- 앱 렘더링
    - root.render(...) 에서는 React.StrictMode 로 감싼 App 컴포넌트를 루트 엘리먼트에 렌더링합니다.
    - StrictMode 는 개발 모드에서 추가적인 검사와 경고를 수행하여 애플리케이션의 잠재적인 문제를 찾고 알려줍니다.


<br>

### 🧷 Props

- `src/Button.js`

```jsx
import PropTypes from "prop-types";
function Button(props) {
    return <button>{props.label}</button>;
}
Button.propTypes = {
    label: PropTypes.string.isRequired,
}
export default Button;
```

- `npm i prop-types` : 전 게시글과 동일
- `import PropTypes from "prop-types"` 로 유효성 검사를 위한 라이브러리를 불러온다.
    - `Button.propTypes = {...}` 원하는 유효성을 실시한다.

<br>

### 🧷 css

- `src/Button.module.css`

```css
.btn {
    color: white;
    background-color: black;
}
```

<br>

- `src/Button.js`

```jsx
...
import styles from "./Button.module.css";
function Button(props) {
    return <button className={styles.btn}>{props.label}</button>;
}
...
```

<br>

- `src/App.module.css`

```css
@import url('https://cdn.rawgit.com/moonspam/NanumSquare/master/nanumsquare.css');

.title {
    font-family: 'NanumSquare';
    font-size: 50px;
}
```
- 내가 좋아하는 폰트인 <span style="background-color: green">네이버의 나눔스퀘어</span>를 적용했다.

<br>

- `src/App.js`
```jsx
import styles from "./App.module.css";
function App() {
  return (
    <div>
      <h1 className={styles.title}>Hello</h1>
      <Button label={"Click Me!"} />
    </div>
  );
}
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/5df470c7-e80c-442e-94cc-304bfa6530c9){: width=100% height=100% .normal} -->


<br>

### 🧷 React Hook

<br>

#### 📌 useEffect VS useMemo

- `useEffect` : 컴포넌트가 처음 렌더링될 때만 실행
API 호출을 통해 데이터를 가져올 경우
- `useMemo` : 데이터가 변경되지 않으면 데이터 처리 로직을 다시 실행하지 않도록 최적화한다.
    - render 시 데이터가 같다면 render 하지 않음

<br>

```jsx
function App() {
  const [counter, setCounter] = useState(0);
  const handleCounter = () => setCounter((prev) => prev + 1);
  console.log("API");
  return (
    <div>
      <Button label={"Click Me!"} handleCounter={handleCounter} />
      <p>Counter: {counter}</p>
    </div>
  );
}
```
<!-- ![gif](https://github.com/JEONGSUJONG/readme-main/assets/142254876/79edf51d-bb1e-456b-af96-d19414db4098){: width=100% height=100% .normal} -->

- 이와 같이 Click Me! 를 클릭하면 App 함수가 계속 호출되는 것을 볼 수 있다.
    - 즉, API 호출한 컴포넌트일 경우 불필요한 API 호출을 계속 하게된다.

<br>

```jsx
function App() {
  ...
  console.log("State Changed ...");
  useEffect(() => {
    console.log("CALL THE API ...")
  }, [])
  return (
    ...
  );
}
```
<!-- ![gif](https://github.com/JEONGSUJONG/readme-main/assets/142254876/ffbb5a4e-6192-4673-8e22-897ab0b2a6d6){: width=100% height=100% .normal} -->

- `useEffect` 를 사용하지 않은 것은 state가 변경될 때마다 계속 실행되고 `useEffect` 를 사용한 것은 단 한번만 실행됨을 확인 할 수 있다.

<br>

#### 📌 useEffect

```jsx
useEffect(() => {
  console.log("I called 'API' once.")
}, []);

useEffect(() => {
  if (keyword !== "" && keyword.length > 5) {
    console.log("I run when 'keyword' changes.");
  }
}, [keyword]);

useEffect(() => {
  console.log("I run when 'counter' changes.")
}, [counter]);
```

<!-- ![gif](https://github.com/JEONGSUJONG/readme-main/assets/142254876/ad5c173a-9d2d-4220-b943-df5d8d3495e6){: width=100% height=100% .normal} -->

- `API` : `[]` 는 빈 배열이므로 React 가 변화를 감지하지 않는다.
- `keyword` : `[keyword]` 는 keyword를 감지하게 되는데 조건이 있으니 true 일 경우 useEffect가 실행됨을 볼 수 있다.
- `counter` : `[counter]` 가 변화할 때만 실행된다.
- 배열에 `[keyword, counter]` 과 같이 명시해주면 두 개 다 변화를 감지하여 실행한다.

<br>

> index.js 의 React.StrictMode 는 앞서 설명한 검사를 실시하는데 해당 태그로 감싸져 있는 경우 자손까지 검사하므로 console 이 두 번씩 찍히는 것을 확인할 수 있다.
{: .prompt-tip }

#### 📌 Clean-up Function

```jsx
function Hello() {
  useEffect(() => {
    console.log("CREATED 😀")
    return () => console.log("DESTROYED 😥")
  }, []);
  return <h1>Hello</h1>
}
```

- `useEffect` 혹은 컴포넌트가 처음 렌더링될 때 (`[]` 빈 배열 이므로) 한 번 실행된다.
- 첫 번째 콜백 함수 내부에서 `CREATED 😀` 가 출력되고 두 번째 콜백 함수는 `useEffect` 의 반환값으로서 컴포넌트가 언 마운트 되기 전(다음 렌더링이 일어나기 전)에 `DESTROYED 😥` 가 출력된다.