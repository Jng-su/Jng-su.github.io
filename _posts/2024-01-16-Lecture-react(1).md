---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (01)
author: JEONGSUJONG
date: 2024-01-16 00:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, ott]
tags: [react]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/a7d1fbfa-583b-40c9-b3be-1fc0e42ba1e0
---

<br>

> ## Vanilla VS React

<br>

### 🧷 vanilla js

```jsx
<body>
    <span>Total Click: 0</span>
    <button id="btn">Click me</button>
</body>
<script>
    let counter = 0;
    const button = document.querySelector("#btn");
    const span = document.querySelector("span");
    function handleClick() {
        counter += 1;
        span.innerText = `Total Click: ${counter}`;
    }
    button.addEventListener("click", handleClick);
</script>
```

1. Vanilla JS에서는 버튼 클릭에 따른 상태 변화를 관리하기 위해 `counter` 변수를 사용한다.
2. `handleClick` 함수에서 `counter`를 증가시키고, 해당 값을 DOM에 직접 업데이트하여 화면에 표시합니다.
3. 상태 변화와 화면 업데이트가 간단하지만, 복잡한 상태 관리에는 한계가 있다.

<br>

### 🧷 React

- React 이해하기

```jsx
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
```

- React component를 import 했기 때문에 `createElement function` 을 가진 React object 에 접근 할 수 있다.
  - 예를 들어, `React.createElement("span")` 를 사용하면 html element를 생성할 수 있다. (개꿀)
  - react-dom : React element들을 HTML body에 둘 수 있게 해준다.
    - react : interactive 한 UI 를 만들 수 있다 (like 엔진)
    - react-dom : React element를 HTML에 두는 역할을 한다. ( `ReactDOM.render(span, Where To Put span?)` : 사용자에게 보여준다.)
      - 실제로는 이렇게 사용안하지만 흐름을 이해하기 위함.

```jsx
<body>
    <div id="root"></div>
</body>

<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script>
    const root = document.querySelector("#root");
    const span = React.createElement("span", { id: "sexy-span" });
    ReactDOM.render(span, root);
</script>
```

- `root` 안에 `span` element를 넣는다(render).

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/404b4b0d-cab6-4e20-9967-b60a2956f0f4){: width="400" height="250" .normal} -->

<br>

#### 📌 React Power

```jsx
const root = document.querySelector("#root");
const Title = () => (
  <h3 id="title" onClick={() => console.log("Stop Bother Me")}>
    Hello I'm title
  </h3>
);

const Btn = () => (
  <button
    onClick={() => {
      console.log("Clicked");
    }}
  >
    Click Me
  </button>
);

const Container = () => (
  <div>
    <Title />
    <Btn />
  </div>
);

ReactDOM.render(<Container />, root);
```

- Arrow Function , 함수를 사용하여 각 컴포넌트를 정의한다.
- `const Container` 역시 함수형으로 정의한다.
- `ReactDOM.render(<Container />, root)` : `Container` 함수를 렌더링한다.
- 함수를 정의하기 위해서 첫 글자는 대문자로 유지하여야 한다. (`<Title />`,`<Btn />`)

<br>

> ## State

- 앞선 Vanilla js 에서의 목표였던 "Counter" 를 증가시키는 코드를 작성해 보려고 한다.

```jsx
function App() {
  const [counter, setCounter] = React.useState(0);
  function onClick() {
    setCounter((current) => current + 1);
  }
  return (
    <div>
      <h1>Total Click: {counter}</h1>
      <button onClick={onClick}>Click Me</button>
    </div>
  );
}

const root = document.querySelector("#root");
ReactDOM.render(<App />, root);
```

- `const ["변수명", "사용될 한수"] = React.useState("변수의 시작 값")` : 사용될 함수는 `set변수명`
- 함수 안에 "사용될 함수" 작성.

```jsx
function onClick() {
  setCounter((current) => current + 1);
}
```

- `current` 함수 사용 : 현재 state를 얻는다.

1. React에서는 `React.useState` 훅을 사용하여 상태를 선언한다. 여기서는 `counter`라는 상태와 이를 업데이트하는 함수 `setCounter`를 선언합니다.
2. `onClick` 함수에서 `setCounter`를 사용하여 상태를 업데이트하면 React는 자동으로 화면을 리렌더링합니다.
3. React의 상태 관리는 더 간편하며, 컴포넌트 간의 상태 전달이나 복잡한 상태 로직을 다룰 때 효과적이다.

React를 사용하면 상태 변화가 간편하고, React의 가상 DOM 덕분에 효율적으로 렌더링된다. 컴포넌트 기반의 아키텍처로 코드를 직관적 볼 수 있다.
