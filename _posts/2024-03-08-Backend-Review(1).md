---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(1)-Manage Project
author: JEONGSUJONG
date: 2024-03-08 00:00:00 +0800
toc: true
pin: false
published: true
categories: [backend, Node.js]
tags: [node.js]
# image: https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/63a46f26-e1ae-489a-a5ce-154f4d4aa987
---

<br>

> ## Node.js

- Node.js는 서버 측 Javascript 실행 환경으로, Google의 V8 Javascript 엔진을 기반으로 한다.
- Node.js는 비동기 기반의 서버를 구축할 수 있고 단일 스레드로 여러 요청을 처리할 수 있는 능력을 제공한다.

<br>

### NPM (Node Package Manager)

- 내가 Node를 선택한 큰 이유 중 하나이다. 다른 훌륭한 개발자들이 저장소에 올려둔 기능들을 가져와서 사용가능하다. 👍
- 이미 구현된 많은 패키지들을 가져와서 사용하므로 프로젝트의 개발 속도를 높인다.
- 코드의 재사용성, 의존성 관리 등 간편하게 사용할 수 있다.

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/fc7cae30-44b7-4f29-8e24-37877184e644) -->

<br>

| 명령어                           | 설명                                 |
| -------------------------------- | ------------------------------------ |
| npm install                      | 설치된 패키지 모두 설치              |
| npm install [package]            | 패키지 추가                          |
| npm install --save-dev [package] | 빌드에 포함되지 않도록 패키지 추가   |
| npm uninstall [package]          | 패키지를 제거                        |
| npm update                       | 모든 패키지를 최신 버전으로 업데이트 |
| npm install --global [package]   | 패키지를 글로벌로 설치 (본인 노트북) |
| npm init                         | package.json을 초기화                |

<br>

### yarn

- NPM 보다 속도가 약간 빠르다. `npm install --global yarn`

<br>

| 명령어                      | 설명                                 |
| --------------------------- | ------------------------------------ |
| yarn / yarn install         | 설치된 패키지 모두 설치              |
| yarn add [package]          | 패키지 추가                          |
| yarn add -dev [package]     | 빌드에 포함되지 않도록 패키지 추가   |
| yarn remove [package]       | 패키지를 제거                        |
| yarn upgrade                | 모든 패키지를 최신 버전으로 업데이트 |
| yarn --global add [package] | 패키지를 글로벌로 설치 (본인 노트북) |
| yarn init                   | package.json을 초기화                |

<br>

### package.json

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/0105ce84-78b2-4a74-a677-bd6922f6afec) -->

- `node_modules` : 다른 개발자들이 원격 저장소에 올려둔 패키지들을 내려받아 저장하는 폴더 📁
- `package.json` : Node.js 프로젝트의 구성 및 의존성을 정의하는 파일

<br>

#### --- Assignment ---

- package.json 생성 후 express 설치하기

```
npm install --global yarn
```

```
yarn init
```

```
yarn add express
```

<br>

### Transpile

- 모든 브라우저 등이 최신 문법을 지원하지 않을 수 있다.
- 어떤 특정 언어로 작성된 소스 코드를 다른 소스 코드로 변환하는 행위
- Ex : ES6 이상 -> ES5 이하로 변환하는 과정
  - NodeJS는 최신 Javascript을 지원하므로 ES6 이상을 사용한다.

### Babel

- ES6 -> <U>BABEL</U> -> ES5

- `yarn add -D @babel/core @babel/cli @babel/node @babel/preset-env`
  - `@babel/core` : 바벨을 사용한다.
  - `@babel/cli` : command line 을 통해 코드를 transpile
  - `@babel/node` : ES6로 작성된 노드 코드를 실행 (성능 저하가 있으므로 `-D` 개발 환경에서만 사용)
  - `@babel/parse-env` : 프리셋을 통해 간단히 바벨 트랜스 파일링 설정

<br>

- package.json

```json
  "scripts": {
    "test" : "node index.js",
    "dev" : "babel-node src/index.js"
  }
  "babel" : {
    "presets" : [
      "@babel/preset-env"
    ]
  }
```

<br>

#### --- Assignment ---

- babel 설치하고 폴더에 import 해보기

```
├─ node_modules
├─ src
    ├─ user
    │   ├─ index.js
    │   └─ user.js
    └─ index.js
├─ yarn.lock
└─ package.json
```

<br>

- src/user/user.js
```javascript
export const users = [
    {
        id: 1,
        name: "jeongsujong",
    },
]
```

<br>

- src/user/index.js
```javascript
export { users } from "./user";
```

<br>

- src/index.js
```javascript
import { users } from "./user";
console.log("Users : " , users);
```