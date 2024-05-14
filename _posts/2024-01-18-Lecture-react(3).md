---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (03)
author: JEONGSUJONG
date: 2024-01-18 00:00:00 +0800
toc: true
pin: false
categories: [lecture, ReactJS로 영화 웹 만들기]
tags: [react]
image: https://github.com/JEONGSUJONG/Readme_main/assets/142254876/7dd6d929-f416-492a-b255-f17f99c5b5a7
---

<br>

> ## Props

- Props (properties) : 부모 컨포넌트에서 자식 컴포넌트로 데이터를 전달 할 때 사용되는 객체.
  - React 컴포넌트는 재사용 가능하므로 컴포넌트 간의 데이터를 주고 받기 위해 `porps` 가 사용된다.

```jsx
function Btn(props) {
  return <button>{props.btnName}</button>;
}

function App() {
  return (
    <div>
      <Btn btnName="A Btn" />
      <Btn btnName="B Btn" />
    </div>
  );
}
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/92149c24-2571-4c06-be56-fa4bada2931c){: width="400" height="250" .normal}

<br>

- `function Btn(props)` : `btnName` 이라는 `props` 를 받아와서 렌더링 한다.
- `function App()` : 각 두 개의 `Btn` 컴포넌트를 렌더링 하고 있다. 각각 `btnName` 이라는 `props` 를 전달한다.
- `props` 를 통해 데이터를 전달함으로써, React 애플리케이션은 부모-자식 간의 효율적인 데이터 흐름을 유지하면서 컴포넌트를 조직하고 재사용할 수 있다.

<br>

### 🧷 Props로 함수 보내기

```jsx
function App() {
  const [valueA, setValueA] = React.useState("A Btn");
  const [valueB, setValueB] = React.useState("B Btn");

  const handleBtnA = () => {
    setValueA((prevValue) => (prevValue === "A Btn" ? "B Btn" : "A Btn"));
  };

  const handleBtnB = () => {
    setValueB((prevValue) => (prevValue === "A Btn" ? "B Btn" : "A Btn"));
  };
  return (
    <div>
      <Btn btnName={valueA} handleBtn={handleBtnA} />
      <Btn btnName={valueB} handleBtn={handleBtnB} />
    </div>
  );
}
```

- `App`컴포넌트 : 상태값 `valueA` 와 `valueB` 가 있다. 각각 상태에 대한 업데이트 함수 `handleBtnA` 와 `handleBtnB` 를 정의한다.
- `Btn` 컴포넌트에 각각 상태와 업데이트 함수(`btnName` 과 `handleBtn`)를 props로 전달한다.

<br>

```jsx
function Btn({ btnName, handleBtn }) {
  return <button onClick={handleBtn}>{btnName}</button>;
}
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/bec637de-a6c4-480c-981a-d39fc6bc687e){: width="400" height="250" .normal}

- `Btn` 컴포넌트는 `btnName`과 `handleBtn`이라는 두 개의 props를 받아와 버튼을 렌더링합니다.
- 버튼 클릭 시, 전달된 `handleBtn` 함수가 실행

#### 📌 props TIP

```jsx
function Btn({ btnName, handleBtn }) {
  return <button onClick={handleBtn}>{btnName}</button>;
}
```

```jsx
return (
  <div>
    <Btn btnName={valueA} handleBtn={handleBtnA} />
    <Btn btnName={valueB} handleBtn={handleBtnB} />
  </div>
);
```
- 이와 같이 `btnName`, `handleBtn` 으로 props를 받을 수 있지만 

```jsx
function Btn(props) {
  return <button onClick={props.handleBtn}>{props.btnName}</button>;
}
```
- 복잡할 경우 `props`로 받고 필요한 것만 꺼내먹기(?)도 가능하다.


<br>

- 👀 Recap
  - App 컴포넌트에서는 두 개의 상태와 각각의 상태를 토글하는 함수를 정의합니다.
  - 이러한 함수들을 Btn 컴포넌트에 props로 전달하여 버튼 클릭 시에 상태를 변경할 수 있도록 합니다.
  - Btn 컴포넌트에서는 받은 handleBtn 함수를 클릭 이벤트에 연결하여 버튼 클릭 시에 함수가 실행되게 합니다.
  - 이렇게 하면 두 개의 버튼이 각각 다른 상태를 가지고, 클릭할 때마다 해당 상태가 토글됩니다.

<br>

### 🧷 React.memo

- 불필요한 re-render는 `React.memo()` 로 관리할 수 있다.
- 부모 컴포넌트의 state를 변경하면 자식 컴포넌트들도 Re-render가 일어난다
  - `React.memo()` 로 prop의 변경이 일어난 부분만 렌더링 시킬 수 있다.
  - 아주 많은 자식 컴포넌트를 가지고 있는 부모 컴포넌트일 경우 사용하기 적합

```jsx
const MemoizedComponent = React.memo(MyComponent);
```

- 컴포넌트가 `React.memo()` 로 wrapping 되면 React는 해당 컴포넌트를 렌더링하고 결과를 Memoizing 한다.
- 다음 렌더링이 발생할 때 props 가 이전과 동일하다면 React는 메모이징된 결과를 재사용하여 불필요한 렌더링을 방지한다.

<br>

### 🧷 Props Type

```jsx
function App() {
  return (
    <div>
      <Btn btnName="Save Changes" />
      <Btn btnName={10} />
    </div>
  );
}
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/76b11fc6-b1bd-45fe-b04b-779cf74f6baf){: width="400" height="250" .normal}

- React는 props를 잘 못 넘겨도 확인할 수 없는 문제점이 존재
- 이런 문제를 해결하기 위해 PropTypes 라는 모듈의 도움을 받을 수 있다.
  - type 과 다르게 입력될 경우 경고(Warning)을 뜨게 하고 prameter 에 값을 넣지 않는 경우 경고 메시지를 띄울 수 있다.

<br>

```jsx
<script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>

...

Btn.propTypes = {
    btnName: PropTypes.string.isRequired
}
```

- `btnName` 을 string 으로 선언하면

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/6a1a1076-faf3-4d7a-8656-2ed7283e2f00){: width=100% height=100% .normal}

- 에러로 알려준다..!!
- `isRequired` 는 필수요소로 입력되어야하는 값을 명시해준다.
  - 입력이 없으면 에러로 알려줌.

<br>

#### 📌 Setting props default value

```jsx
function App() {
  return (
    <div>
      <Btn btnName="A Btn" />
      <Btn />
    </div>
  );
}
```

- 만약에 `btnName` 을 아무 값도 안넣어주면 당연히 빈 버튼이 나온다.

<br>

```jsx
function Btn({ btnName = "B Btn" }) {
  return <button>{btnName}</button>;
}
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/5ab73251-c4d0-40a9-a48f-500bd0e0fd4f){: width=100% height=100% .normal}

- 하지만, 위와 같이 `btnName` 을 명시해주면 놀랍게도 값이 없으면 명시된 값을 넣어주는 것을 확인할 수 있다...!
