---
# 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-BE (1)
author: JEONGSUJONG
date: 2024-01-31 01:00:00 +0800
toc: true
pin: false
published: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## npm init & Install Express

- `npm init`
  - package.json : 프로젝트의 정보와 프로젝트에서 사용 중인 패키지의 의존성을 관리하는 곳
- `npm install express`
  - express : Node.js 의 API를 단순화하고 유용한 기능들을 더 추가 시켜 Node.js 를 더 편리하고 유용하게 사용할 수 있게 해주는 모듈
- index.js 파일 생성
  - Node.js 에서 진입점이 되는 파일

<br>

### 🧷 Connecting Server

```javascript
const express = require("express");
const dotenv = require("dotenv");

dotenv.config();

const app = express();

app.get("/", (req, res) => {
  res.send("Hello World");
});

//PORT
const PORT = process.env.PORT;

app.listen(PORT, () => {
  console.log(`Listening on ${PORT}`);
});
```

`node index.js`

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/0b28551e-13ba-41f1-bf23-9974113f74be){: width=100% height=100% .normal}

<br>

- `npm install -D nodemon : nodemon` 은 Node.js 애플리케이션의 코드 변경을 감지하고 자동으로 서버를 다시 시작해주는 도구
- package.json

```json
  "scripts": {
    "dev": "nodemon src/index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

`npm run dev`

<br>

> ## BackEnd Folder Structure

```
├─node_modules
├─src
    ├─middleware
    │   └─auth.js
    ├─models
    │   └─User.js
    ├─routes
    │   └─users.js
    └─index.js
├─public
    └─//static 파일
├─.env
├─.gitignore
├─package-lock.json
└─package.json
```

<br>

> ## Install Package

- `npm install bcryptjs` : bcryptjs 비밀번호를 해싱을 수행하기 위한 라이브러리
- `npm install jsonwebtoken` : jsonwebtoken JSON 형식의 토큰을 생성하고 검증하기 위한 라이브러리. 주로 (Authentication) 을 구현할 때 사용되며 특히, 웹 토큰 (JSON Web Token)을 생성하여 사용자의 세션 상태를 유지하거나 검증할 수 있게 한다.
- `npm install cors` : Express 애플리케이션에서 클라이언트와 서버 간의 HTTP 요청에 응답하고 자원 요청을 허용하는 미들웨어
- `npm install mongoose` : MongoDB와 상호 작용하기 위한 Node.js 라이브러리.

<br>

> ## express.static

- `express.static()` : 이미지, CSS 파일 및 Javascript 파일과 같은 정적 파일을 제공하려면 Express의 express.static 내장 미들웨어 기능을 사용할 수 있다.

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/91ae5eae-806b-46c1-b35d-0a3a660cf579){: width=100% height=100% .normal}

<br>

- index.js

```javascript
const path = require("path");

// public 폴더 안의 정적 파일 가져오기
app.use(express.static(path.join(__dirname, "../public")));
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/a8029796-30c4-48e9-8eb6-3aa8b6cd800a){: width=100% height=100% .normal}

- `__dirname` : 현재 파일이 위치한 디렉토리를 의미
    - `path.join` 을 사용하여 정적 파일이 위치한 디렉토리 경로를 지정합니다.
- `"../public"` 은 정적 파일들이 위치한 디렉토리


> app.use("가상경로", express.static("public")); : 가상경로로 사용할 수 있지만 노드 프로세스를 시작하는 디렉토리는 상대적이기에 위의 예시인 절대 경로로 사용하는 것을 권장.
{: .prompt-tip }


<br>

> ## Cors

- Server와 Client 간 Port가 다르거나 도메인이 다르면 즉, Origin이 다르면 Request 를 보낼 수 없다.
    - Why? 보안을 위한 Same Origin Policy 정책 때문 (동일 출처 정책)
- Cross-Origin Resource Sharing 를 사용하면 된다.
`app.use(cors())`

<br>

> ## Express.json

```javascript
app.use(express.json());

app.post("/", (req, res) => {
  console.log(req.body);
  res.json(req.body);
});
```

![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/f2a43553-bea8-44ca-941c-65220e6d335e){: width=100% height=100% .normal}

<br>

> ## mongoose

- Mongoose : 데이터를 만들고 관리하기 위해서 먼저 Schema 를 만들고 그 스키마로 모델을 만든다. 몽구스는 MongoDB 를 쓸 때 사용해도 되고 안 써도 되는 선택사항

<br>

1. 스키마를 생성한다
2. 스키마를 이용하여 모델을 만든다
3. 모델을 이용하여 데이터를 CRUD 할 수 있다.

```javascript
const mongoose = require("mongoose");

const product = new mongoose.Schema({
  name: {
    type: String,
    required: true
  }
  price: {
    type: Number
  }
  ...
})

const Product = mongoose.model("Product", productSchema);
module.exports = Product;
```

- 애플리케이션 계층에서 특정 스키마를 적용
    - 모델 유효성 검사
    - MongoDB 작업을 쉽게 하기 위한 기타 기능
- Schema : Mongoose 스키마는 문서 (Document)의 구조, 기본값, 유효성 검사를 정의 (default: 0, required: true/false)
- Model : Mongoose 모델은 레코드 생성,쿼리,업데이트,삭제 등을 위한 데이터 베이스 인터페이스 제공

<br>

### 🧷 MongoDB Atlas Cloud Service

[https://www.mongodb.com/ko-kr](https://www.mongodb.com/ko-kr)

- MongoDB Atlas 프로젝트 생성
    - MongoDB Atlas에 로그인
    - Create New Project
    - Build a Database
    - 프로젝트에서 "Database Access"로 이동하여 username과 password 가진 새 데이터베이스 사용자를 생성 (기억해야함)

```javascript
const mongoose = require("mongoose");

// mongoose
mongoose
  .connect(process.env.MONGO_URL, {
    dbName: process.env.MONGO_DB_NAME,
  })
  .then(() => console.log("MongoDB Connected"))
  .catch((err) => console.log(err));
```

<br>

- 데이터 베이스 구축
    -  Atlas -> Deployment/Database -> Drivers
    -  `Add your connection string into your application code` URL 복사
    - .env 파일에 넣기 
        -  URL 안에 `<username>:<password>` DatabaseAccess 설정한 username과 password 넣어주기

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/e69972af-2f92-47d0-92ad-c228bdbc70db){: width=100% height=100% .normal}

정상적으로 연결되었다..!

- MongoDB Compass 에서도 동일한 URL 적용

<br>

#### 📌 Create User Schema

- models/User.js

```javascript
const mongoose = required("mongoose");

const userSchema = mongoose.Schema({
  name: {
    type: String,
    maxLength: 50,
  },
  email: {
    type: String,
    trim: true, // 문자열 양 끝 공백 제거
    unique: 1, // 중복된 값 허용 x
  },
  password: {
    type: String,
    minLength: 5,
    required: true,
  },
  role: {
    type: Number,
    default: 0,
  },
  image: {
    type: String,
  },
});

const User = mongoose.model("User", userSchema);

module.exports = User;
```

<br>

> ## Express Error Handling

- 기본 라우트에서 에러 발생시키기
```javascript
app.get("/", (req, res) => {
  throw new Error("This is Error");
});
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/9b311df2-bd1d-4af6-b232-9709aa981567){: width=100% height=100% .normal}

<br>

- 실행하면 바로 서버가 다운(Crash) 된다 (에러 처리 미들웨어 필요)

```javascript
app.get("/", (req, res) => {
  throw new Error("This is Error");
});

// 에러 처리기
app.use((error, req, res, next) => {
  res.send(error.message);
});
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/064f979b-928b-423f-bfa6-48648bd97a6c){: width=100% height=100% .normal}

- 위와같으면 서버가 다운 되지 않고 에러 메시지를 보여준다.
- 하지만, 비동기 요청으로 인한 에러는 에러 처리기에서 못 받는다.

<br>

```javascript
app.get("/", (req, res) => {
  setImmediate(() => {
    throw new Error("This is Error");
  });
});

app.use((error, req, res, next) => {
  res.send(error.message);
});
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/67139389-371a-4c93-9d63-1cab76a65399){: width=100% height=100% .normal}

<br>

### 🧷 How to solve?

- `next` 사용

```javascript
app.get("/", (req, res, next) => {
  setImmediate(() => {
    next(new Error("This is Error"));
  });
});

app.use((error, req, res, next) => {
  res.status(500).send(error.message || 'Internal Server Error');
});
```