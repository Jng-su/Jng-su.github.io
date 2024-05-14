---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(2)-Express
author: JEONGSUJONG
date: 2024-03-08 00:01:00 +0800
toc: true
pin: false
published: false
categories: [backend, Node.js]
tags: [express]
image: https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/63a46f26-e1ae-489a-a5ce-154f4d4aa987
---

<br>

> ## Express (1)

- NodeJS 를 위한 빠르고 개방적인 간결한 웹 프레임워크 ( Http + Node.js + Library )
- `yarn add express`

<br>

### HTTP Req,Res

- Client 와 Server는 어떤 방식으로 소통을 할까? ( request -> response )
- 바로 <U>HTTP Protocol</U> 방식으로 소통을 하게 된다. ( Ex. https://www.youtube.com )

![HTTP-Protocol](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/291661a6-f263-4bce-a0a4-3f02c76cfaac)

- HTTP Request 요청이 Client로 부터 오면 서버는 <U>OPEN</U> 되고 HTTP Response 응답을 Server가 보내주면 Server는 다시 <U>CLOSE</U> 하게 된다.

<br>

![httpstructure](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/85377395-0d7e-4779-977a-75ff63179020)

- `start line`
  - HTTP request Message의 시작 라인 ( HTTP method , Request target , HTTP version )
    - `http method` : GET , POST , PUT, DELETE
    - `Request target` : HTTP Request의 목적지
    - `HTTP version` : version에 따라 Request 메시지 구조나 데이터가 다를 수 있으므로 version을 명시해야함.
- `header`
  - Additional information 을 담고 있는 부분 (request message length , content length 등 Key:Value 형태로 구성됨.)
  - 크게 general header , request header , entity headers 3가지로 구성
    - `Host` : 요청하려는 서버 호스트 이름과 포트번호
    - `User-agent` : Client 프로그램 정보 (클라이언트 프로그램에 맞는 최적의 데이터를 보내기 위함)
    - `Referer` : 바로 직전에 머물렀던 웹 링크 주소
    - `Accept` : 클라이언트가 처리 가능한 미디어 타입 종류 나열
    - `Authorization` : 인증 토큰
    - `Origin` : 서버로 Post 요청 시 요청이 어느 주소에서 시작되었는지 알려줌. ( 요청을 보낸 주소와 받는 주소가 다르면 <U>CORS(Cross-Origin Resource Sharing)</U> 에러 발생)
    - `Cookie` : Key:Value 로 표헌
- `body`
  - HTTP Request 가 전송하는 데이터를 담고 있는 부분
  ```json
  {
    "id": 123456789,
    "name": "JEONGSUJONG",
    "birth": "971228"
  }
  ```

<br>

### REST API

- REST API 란 REST의 원리를 따르는 API를 의미한다.
  1. URL는 동사보다 명사를 대문자보다는 소문자를
  2. 마지막에 슬래시 `/` 를 포함하지 않는다.
  3. 언더바 대신 하이폰을 사용한다.
  4. 파일확장자는 URL에 포함하지 않는다. (jpg, png, gif ...)
  5. 행위를 포함하지 않는다. (delete-post, update-post ...)
  6. 영어는 복수형으로 사용

<br>

#### REST - Resource(자원)

![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/f41eedf0-186b-457e-994e-e7991e3b66d8){: width=100% height=100% .normal}

<br>

HTTP URL : 서버에 요청을 보내는 주소 (`localhost:5000`)

```
http://localhost:5000/users/:id?name=정수종&age=28
```

| 명령어               | 설명                   |
| -------------------- | ---------------------- |
| `http`               | protocol               |
| `localhost`          | host                   |
| `5000`               | port ( `443` : https ) |
| `users/:id`          | path                   |
| `:id`                | path variable          |
| `name=정수종&age=28` | query                  |

<br>

<br>

#### REST - Verb(행위)

- HTTP Method

  | -------------------- | ---------------------- |
  | `Create` | POST |
  | `Read` | GET |
  | `Update` | PUT, PATCH |
  | `Delete` | DELETE |

<br>

#### REST - Representations(표현)

- 요청과 응답의 형식을 통해 자원을 표현한다.
- Client 와 Server 간의 상호작용을 가능하게 하고 JSON 형식의 데이터를 요청하면 서버는 해당 형식으로 응답하여 클라이언트가 데이터를 쉽게 처리할 수 있도록 한다.
- JSON, XML, TEXT
  - JSON : 데이터를 표현하기 위한 경량의 데이터 교환 형식. 특히, Javascript에서 쉽게 파싱할 수 있다.

```json
{
  "name": "JEONGSUJONG",
  "age": 28,
  "city": "Seoul",
  "isStudent": false,
  "hobbies": ["coding", "swim"],
  "address": {
    "street": "Gwanak",
    "zipCode": "00000"
  }
}
```

<br>

> ## Express (2)

### core modules

- `express` : Node.js용 웹 애플리케이션 프레임워크
- `http` : HTTP 서버 모듈
- `path` : 파일 및 디렉토리 경로와 관련된 경로 유틸리티

### commonly used libraries

- `body-parser` : 들어오는 요청 본문을 파싱하기 위한 미들웨어
- `cors` : 접근 가능한 도메인을 제한
- `helmet` : Http 헤더 설정을 통해 보안 강화
- `dayjs` : 날짜 관련 작업을 할 때 사용
- `nodemon` : 수정된 코드에 따라 다시 서버를 실행
- `jsonwebtoken` : JSON Web Token (JWT) 로그인한 유저 정보를 토큰으로 만들기 위해 사용
- `bcrypt` : 비밀번호 해싱 및 암호화를 위한 라이브러리
- `mongoose` : Node.js 용 MongoDB 객체 모델링 도구
- `axios` : 다른 서버에 요청을 보낼 때 사용 <U>(Kakao/Naver - Login/Map)</U>

<br>

```terminal
yarn add -D nodemon
```

- 개발환경에서만 사용

<br>

```terminal
yarn add cors helmet dayjs bcrpyt jsonwebtoken
```

<br>

```javascript
import express from "express";
import cors from "cors";
import helmet from "helmet";
import dayjs from "dayjs";

const app = express();

app.use(cors());
app.use(helmet());
const currentTime = dayjs(new Date()).format("YYYY.MM.DD");
```

```javascript
  "scripts": {
    "dev": "nodemon --exec babel-node src/index.js"
  },
```

<br>

```javascript
import bcrypt from "bcrypt";
import jwt from "jsonwebtoken";

const tokenPayload = {
  userId: "12345",
  role: "admin",
};

const hashedPassword = bcrypt.hashSync(password, 10);
const token = jwt.sign(tokenPayload , "helloworld", {expiresIn: "1h"});
```
- `hashSync(password, NUMBER)` 에서의 `NUMBER` 는 솔트(salt) 라운드를 지정해준다.
    - salt는 해시 함수에 사용되는 반복 횟수를 의미하며 라운드가 높을 수록 해시 함수를 실행하는 데 시간이 더 소요되고 보안이 강화된다.

- `jwt.sign("PAYLOAD", "SECRET_KEY", "ADDITIONAL_SETTING")`
    - `PAYLOAD` : JWT 토큰을 전달하는 표준 방식. 객체 혹은 문자열을 사용
        - PAYLOAD는 일반적으로 객체를 전달받고 그 안에 클라이언트에서 확인할 수 있는 정보를 담아서 보내고 decode 과정을 통해 추출할 수 있다.
    - `SECRET_KEY` : JWT 토큰을 서명할 때 사용되는 비밀 키 , 이 키는 토큰의 무결성을 보장하고 토큰의 발행자를 확인하는 데 사용, SECRET_KEY는 토큰을 검증할 때도 사용
    - `ADDITIONAL_SETTING` : 옵션 객체로 토큰에 대한 추가적인 설정을 제공한다.

<br>

> ## Middleware

- Client와 Server 사이에 위치해 요청을 보낼 때 항상 Middleware를 통과한다.

![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/ca2c364f-0005-4b60-85dc-d1c781cab604)

- 중간에 필요한 공통적인 기능을 쉽게 넣을 수 있다.
- 보안상으로 클라이언트와 서버간의 통신을 중간에서 감시하고 조작하는 보안적인 장치를 둘 수 있다.
- 로그인 여부(Token)를 통해 인증된 사용자에게만 허용된 요청을 처리한다.

<br>

### Application Level Middleware

모든 요청이 거쳐가는 미들웨어. 모든 요청에 대해 공통된 작업을 수행하거나 모든 요청에 대한 전처리 및 후처리 작업을 처리한다. 예를들어, `app.use()` 메서드를 사용하여 라우팅 설정 전에 전역으로 등록한다.

### Router Level Middleware

어떤 한 요청, 다른 한 요청에 대해서만 거쳐가는 미들웨어. (GET, PATCH ... 다른 미들웨어를 거치게 할 수 있다.) 예를 들어, `router.use()` 메서드를 사용하여 특정 라우팅 경로에 대한 요청을 처리하기 전에 실행되며, 해당 경로에 대한 요청에만 영향을 미친다.
