---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(3)-REST API
author: JEONGSUJONG
date: 2024-03-08 00:02:00 +0800
toc: true
pin: false
published: false
categories: [tech, node.js]
tags: [restapi]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/580ff546-c5d4-4502-a0c3-489b13a49870
---

<br>

> ## REST API

- express 앱 생성하기

```javascript
// yarn add express
import express from "express";
const app = express();
```

<br>

- Application Level 미들웨어 작성 (모든 요청이 거쳐가는 미들웨어)

```javascript
app.use(express.urlencoded({ extended: true, limit: "700mb" }));
app.use(express, json());
app.use(cors({ origin: "*" }));
app.use(helmet());
```

<br>

> ## app.use()

- Middleware : `app.use(helmet());`
- Router : `app.use("/users", router);`
  - GET , POST , PATCH , DELETE

```javascript
// GET 요청 성공시 : 200
app.get("/users", (req, res) => {});
// POST 요청 성공시 : 201
app.post("/users", (req, res) => {});
// PATCH 요청 성공시 : 204
app.patch("/users/:id", (req, res) => {});
// DELETE 요청 성공시 : 204
app.delete("/users/:id", (req, res) => {});
```

<br>

- express 앱 실행하기

```javascript
app.listen(PORT, () => {
    console.log(`Listening on ${PORT}`);
})
```

<br>

#### --- Assignment ---

`app.use` 를 통해 간단한 미들웨어 구현 후 , `app.get` `app.post` `app.patch` `app.delete` 를 통하여 REST API를 만들어 보시오.

```javascript
app.get("/users", (req, res) => {
  res.status(200).json({ users });
});
```

<br>

```javascript
app.post("/users", (req, res) => {
  const { name, age } = req.body;
  users.push({
    id: new Date().getTime(),
    name,
    age,
  });
  res.status(201).json({ users });
});
```
- `name` 과 `age` 를 params의 body로 받아서 `push` 하는 과정
- 응답으로 201 성공응답과 `push`한 `users` 를 반환해준다.

<br>

```javascript
app.patch("/users/:id", (req, res) => {
  const { id } = req.params;
  const { name, age } = req.body;
  const targetUserIdx = users.findIndex((user) => user.id === Number(id));
  users[targetUserIdx] = {
    id: users[targetUserIdx].id,
    name: name ?? users[targetUserIdx].name,
    age: age ?? users[targetUserIdx].age,
  };
  res.status(204).json({});
});
```
- `/:id` 로 해당 유저의 id 값을 받는다
- id는 params로 들어왔으므로 해당하는 `id`를 저장한다.
- body에 있는 `name` 과 `age` 속성을 저장한다.
- 배열에서 해당 `id`를 가진 사용자의 인덱스를 찾는다.
- 찾은 userIdx를 body로 들어온 `name`과 `age`를 입력받는다.
    - `name` `age` 가 요청에 포함되어있으면 사용하고 없으면 기존 사용자의 정보를 유지한다.

<br>

```javascript
app.delete("/users/:id", (req, res) => {
  const { id } = req.params;
  const deleteUsers = users.filter((user) => user.id !== Number(id));
  users = deleteUsers;
  res.status(204).json({});
});
```
- `/:id` 로 해당 유저의 id 값을 받는다.
- 배열에서 해당 `id`를 가진 사용자를 제외하고 새로운 배열을 생성한다. (즉, filtering 을 한다.)
- `users` 배열을 새로운 배열로 교체한다.