---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FE (1)
author: JEONGSUJONG
date: 2024-01-29 00:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## FolderStructure

- vite를 이용하여 react를 생성할 것이다.
    - `cd FE`
    - `npm init vite`
        - project name : `shoppingmall`
        - select a framework : `React`
        - select a variant : `javascript`
    - `npm install` (node_modules 설치 확인)
    - `npm run dev` (port에서 웹 확인)

```
└─src
    ├─assets
    ├─components
    │   ├─NotAuthRoutes.jsx
    │   └─ProtectedRoutes.jsx
    ├─hooks
    ├─layout
    │   ├─Footer
    │   │   └─index.jsx
    │   └─NavBar
    │       ├─Sections
    │       │   └─NavItem.jsx
    │       └─index.jsx
    ├─pages
    │   ├─LandingPage
    │   │   └─index.jsx
    │   ├─LoginPage
    │   │   └─index.jsx
    │   └─RegisterPage
    │       └─index.jsx
    ├─store
    │   ├─index.js
    │   ├─thunkFunction.js
    │   └─userSlice.js
    └─utils
        └─axios.js
```

<br>

> ## Vite ESLint 설정

`npm install -D vite-plugin-eslint eslint eslint-config-react-app`

<br>

### 🧷 eslint plugin 적용하기

- `vite.config.js`
    - `import eslint from 'vite-plugin-eslint';`
    - `plugins: [react(), eslint()],` : `eslint()` 추가

<br>

- src/.eslintrc 파일 추가
```
{
    "extends" : [
        "react-app"
    ]
}
```

<br>

> ## Tailwind CSS

- VSCode 확장 프로그램 -> Tailwind CSS IntelliSense 설치

<br>

### 🧷 Tailwind

[https://tailwindcss.com/](https://tailwindcss.com/)

- HTML 안에서 CSS 스타일을 만들 수 있게 해주는 CSS 프레임 워크

- Tailwind CSS는 부트스트랩과 비슷하게 m-1, flex 와 같이 미리 세팅된 Utility Class 를 활용하는 방식으로 HTML 에서 스타일이 가능하다.
    - 빠른 스타일링 작업이 가능
    - class 혹은 id 명을 작성하기 위한 고생을 하지 않아도된다.
    - Utilty Class가 익숙해지는 시간이 필요할 수 있지만 IntelliSense 플러그인이 제공돼서 금방 익숙해진다.

<!-- ![gif](https://github.com/JEONGSUJONG/readme-main/assets/142254876/c379482f-b15d-4b12-a98d-16b5ffd8fcce){: width=100% height=100% .normal} -->

<br>

### 🧷 Tailwind 설정하기

[https://tailwindcss.com/docs/guides/vite](https://tailwindcss.com/docs/guides/vite)

- `npm install -D postcss autoprefixer tailwind` 혹은 `npm install tailwindcss --save-dev` 나는 전자의 방법이 안돼서 후자로 하니까 된다..!

- npx tailwindcss init -p
    - postcss.config.js , tailwind.config.js 파일 설치 확인!

- tailwind.config.js
```javascript
content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
],
```

<br>

- index.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

<br>

Tailwind 준비 끝

```jsx
import "./App.css";
function App() {
  return (
    <div className="App">
      <h1 className="text-3xl font-bold underline">
        Hello World!
      </h1>
    </div>
  );
}
export default App;
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/fd7b4e4c-4c22-421a-af86-1b02db5ae967){: width=100% height=100% .normal} -->