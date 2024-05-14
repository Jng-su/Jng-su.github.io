---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(5)-Swagger
author: JEONGSUJONG
date: 2024-03-09 00:00:00 +0800
toc: true
pin: false
categories: [backend, Node.js]
tags: [swagger]
image: https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/63a46f26-e1ae-489a-a5ce-154f4d4aa987
---

<br>

> ## SwaggerUI

- API 문서를 자동으로 생성하고 시각화해주는 도구로써 FE 와 BE 개발자들은 API 요청 및 응답에 대한 정보를 쉽게 찾고 사용할 수 있다.
  - 작년 팀 프로젝트 당시 API 응답, 요청 메소드들을 직접 노션에 정리해서 프론트엔드 분들이랑 공유했던 것을 생각하면 Swagger의 중요성을 알게 되었다. 😭
- Swagger UI를 사용하면 가독성이 높고 이해하기도 쉬움.

<br>

![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/e97c6b45-7993-42e5-8795-b4d45d3141f4)

- Swagger는 모든 문서를 json 형식으로 작성해야하고 각 엔드포인트에 대한 요청과 응답에 대한 정보를 **세부적으로** 기술해야한다.
- 즉, Swagger는 API 문서를 시각적으로 보여주므로 개발자들이 요청과 응답의 구조를 이해하기 쉽고 실수를 방지할 수 있다.

<br>

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "Swagger Petstore",
    "description": "설명 들어가는 곳",
    "version": "1.0.11"
  },
  "servers": [
    {
      "url": "https://petstore3.swagger.io/api/v3"
    }
  ]
}
```

![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/24e964f2-56ac-4e9d-8ad1-ea1d03881dc1)

- `version` : package.json에서의 version과 동일해야함
- `servers` : Server url으로 Deploy시 여러 개의 url이 생길 수 있으므로 배열로 작성

<br>

```json
{
  "paths": {
    "/pet": {
      "post": {
        "tags": ["pet"],
        "summary": "Add a new pet to the store",
        "description": "~~~",
        "operationId": "addPet",
        "consumes": ["appliction/json"],
        "produces": ["appliction/json"],
        "parameters": [],
        "responses": {},
        "security": []
      }
    }
  }
}
```

- `"/pet"` : API 엔드포인트를 나타낸다. 이는 클라이언트가 서버에 요청을 보내는 주소
- `"post"` : HTTP method (GET, POST, PATCH, DELETE ...)
- `parameters` : API 요청에 필요한 매개변수를 정의한다.
- `requestBody` : API 요청 본문에 대한 정보를 정의한다.
- `responses` : API 응답에 대한 정보를 정의한다. 각각의 HTTP 상태 코드(200, 201 ...)에 대한 응답 정보를 추가한다.

<br>

천천히 실습 해보면서 알아가는 게 더 좋을 거 같다. 😀

<br>

### Training Swagger

`yarn add install swagger-ui-express`

<br>

1. 폴더 생성

   ```
   ├─ src
   │    ├─ controllers
   │    │   ├─ users
   │    │   │     ├─ index.js
   │    │   │     └─ swagger.js
   │    │   ├─ swagger.js
   │    │   └─ index.js
   │    ├─ swagger
   │    │   ├─ index.js
   │    │   └─ defaultSwagger.js
   │    └─ index.js
   ```

2. `/swagger/defaultSwagger.js`

   ```javascript
   const defaultSwagger = {
     openapi: "3.0.0",
     info: {
       version: "1.0.0",
       title: "Training swagger",
       description: "Swagger page"
     }
   };

   export default defaultSwagger;
   ```

3. `/controllers/users/swagger.js`

   ```javascript
   export const getUserSwagger = {
     "/detail/:id": {
       tags: ["Users"],
       summary: "유저 상세 조회",
       parmeters: [
         {
           in: "path",
           name: "id",
           required: true,
           schema: {
             type: "number"
           }
         }
       ],
       responses: {
         200: {
           content: {
             "application/json": {
               schema: {
                 type: "object",
                 properties: {
                   user: {
                     type: "object",
                     properties: {
                       id: {
                         type: "number"
                       },
                       name: {
                         type: "string"
                       },
                       age: {
                         type: "number"
                       }
                     }
                   }
                 }
               }
             }
           }
         }
       }
     }
   };
   ```

4. `/controllers/swagger.js`

   ```javascript
   export * as UserSwagger from "./users/swagger";
   ```

<br>

- 여기까지 Users의 getUser에 대한 기본적인 swagger 세팅이 완료되었다. 🥵🥵🥵

<br>

### -----------------------------

- `/swagger/index.js`

  ```javascript
  import * as Swagger from "../controllers/swagger";
  console.log(Swagger);

  // 1) path를 가공하는 코드 작성

  // 2) swagger에 등록할 json 만들기 : defaultSwagger + 앞서 등록한 path

  // 3) swagger에 등록하는 방법
  ```

![image](https://github.com/JEONGSUJONG/JEONGSUJONG/assets/142254876/80efc9df-4e15-4779-8a70-35d2c29321d0)

<br>

- `index.js`

```javascript
import { swaggerDocs, options } from "./swagger";
import swaggerUi from "swagger-ui-express";

// Swagger
app.get("/swagger.json", (req, res) => {
  res.status(200).json(swaggerDocs);
});

// localhost:8000/api-docs
app.use("/api-docs", swaggerUi.serve, swaggerUi.setup(undefined, options));
```

<br>

```javascript
import * as Swaggers from "../controllers/swagger";
import defaultSwagger from "./defaultSwagger";

// 1) path를 가공하는 코드 작성
const { paths } = Object.values(Swaggers).reduce(
  (acc, apis) => {
    const APIs = Object.values(apis).map((api) => {
      return { ...api };
    });
    APIs.forEach((api) => {
      const key = Object.keys(api)[0];
      if (!acc.paths[key]) {
        acc.paths = {
          ...acc.paths,
          ...api
        };
      } else {
        acc.paths[key] = {
          ...acc.paths[key],
          ...api[key]
        };
      }
    });
    return acc;
  },
  { paths: {} }
);

// 2) swagger에 등록할 json 만들기 : defaultSwagger + 앞서 등록한 path
export const swaggerDocs = {
  ...defaultSwagger,
  paths
};

// 3) swagger에 등록하는 방법, 복붙!!!
export const options = {
  swaggerOptions: {
    url: "/swagger.json"
  }
};
```
