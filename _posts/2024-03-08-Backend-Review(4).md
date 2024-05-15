---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(4)-Router
author: JEONGSUJONG
date: 2024-03-08 00:03:00 +0800
toc: true
pin: false
published: true
categories: [backend, Node.js]
tags: [router]
# image: https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/63a46f26-e1ae-489a-a5ce-154f4d4aa987
---

<br>

> ## Router

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/e582602c-6493-495c-8d08-25049af15d17) -->

- Express 애플리케이션에서는 클라이언트의 요청을 처리하기 위해 여러 개의 엔드포인트를 설정하고 관리해야한다.
- 이때 각 엔드포인트 별로 요청을 처리하는 핸들러를 작성하고 이를 ROUTER에 등록하여 관리한다.
- ROUTER는 특정 엔드포인트에 대한 요청을 처리하기 위한 미들웨어 집합이며 ROUTER를 통해 애플리케이션의 코드를 구조화하고 가독성을 높일 수 있다.
- 앞선 실습에선 ROUTER를 사용하지 않았기에 가독성이 떨어졌다. 이를 개선하기 위해 라우터를 사용해야한다.

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/b44f4728-949e-4470-b4ef-82ff1016e78a) -->

```javascript
const UserRouter = Router();
// 모든 유저를 불러오는 API
UserRouter.get("/", (req, res) => {
  res.status(200).json({ users });
});
// 특정 유저를 불러오는 API
UserRouter.get("/detail/:id", (req, res) => {
  const { id } = req.params;
  users.find((user) => user.id === Number(id));
  res.status(200).json({ users });
});
// 유저를 생성하는 API
UserRouter.post("/", (req, res) => {
  const { name, age } = req.body;
  users.push({
    id: new Date().getTime(),
    name,
    age
  });
  res.status(201).json({ users });
});
// 유저 ROUTER 등록하기
app.use("/users", UserRouter);
```

<br>

> ## Class

- 클래스를 사용하면 관련 기능을 하나의 클래스로 구성하여 관리할 수 있다.
- 클래스를 사용하면 유저 관련 로직을 하나의 모듈로 캡슐화하여 재사용성을 높일 수 있다.

```javascript
import { Router } from "express";

// Router
class UserController {
  router;
  path: "/users";
  users = [
    {
      id: 1,
      name: "JEONGSUJONG",
      age: 28
    }
  ];

  // 클래스의 생성자 함수 실행
  constructor() {
    this.router = Router(); // 라우터 등록
    this.init(); // init 등록
  }

  // 생성자를 실행할 때 가장 먼저 실행되는 함수
  init() {
    // 여기서의 this는 클래스 내에서 메서드를 호출할 때 해당 클래스의 인스턴스를 가리킨다.
    this.router.get("/", this.getUsers.bind(this));
    this.router.get("/detail/:id", this.getUser.bind(this));
    this.router.post("/", this.createUser.bind(this));
  }

  getUsers(req, res) {
    res.status(200).json({ users: this.users });
  }

  getUser(req, res) {
    const { id } = req.params;
    // 함수 내에서의 this는 해당 메서드가 속한 클래스의 인스턴스를 가리킨다.
    const user = this.users.find((user) => user.id === Number(id));
    res.status(200).json({ user });
  }

  createUser(req, res) {
    const { name, age } = req.body;
    // 함수 내에서의 this는 해당 메서드가 속한 클래스의 인스턴스를 가리킨다.
    this.users.push({
      id: new Date().getTime(),
      name,
      age
    });
    res.status(201).json({ users: this.users });
  }
}

const userController = new UserController();
export default userController;
```

- 어떤 것을 선택할까?
  - Router : 라우터를 사용하면 각 엔드포인트에 대한 요청을 처리하는 콜백 함수를 간단하게 정의할 수 있다. 또한, 라우터를 사용하면 코드가 간결해지고 가독성이 높아진다.
  - Class : 클래스를 사용하면 관련 기능을 하나의 클래스로 구성하여 관리할 수 있다. 클래스를 사용하면 유저 관련 로직을 하나의 모듈로 캡슐화하여 재사용성을 높일 수 있다.
- `this` 😰
  - 클래스에서의 `this` 는 클래스 내부에서 메서드를 호출할 때 메서드가 속해 있는 객체를 가리킨다. 즉, 클래스의 메서드 내부에서 `this` 를 사용하면 해당 클래스의 인스턴스를 참조한다.
  - 함수에서의 `this` 는 해당 함수가 호출된 컨텍스트를 가리킨다. 하지만, Arrow Function 에서는 외부 함수의 `this`를 그대로 가리킨다.
  - 클래스를 사용할 때는 클래스의 인스턴스를 가리키기 위해 `this` 를 사용하고 라우터를 사용할 때는 콜백 함수의 컨텍스트를 가리키기 위해 `this`를 사용한다.

<br>

- `Context`
  - 코드가 실행되는 환경
  - Javascript에서 함수가 호출될 때, 함수 내부의 `this`는 함수가 호출되는 방법에 따라 다른 객체를 가리킨다. 객체의 메서드로 함수가 호출되면 `this`는 해당 객체를 가리키고, 함수가 독립적으로 호출되면 `this`는 전역 객체를 가리킨다.
- `Instance`
  - 클래스에 의해 생성된 객체를 의미
  - 객체지향프로그래밍에서 클래스는 객체를 생성하기 위한 설계도이다. 이 설계도를 기반으로 실제 객체를 생성할 수 있는데 이렇게 생성된 객체를 인스턴스라고 한다.
  - 예를 들어, `Car` 클래스를 통해 `new Car()`를 호출하여 자동차 객체를 생성할 수 있다.

<br>

### controllers

```
├─ src
    ├─ controllers
    │   ├─ users
    │   │   └─ index.js
    │   └─ index.js
    └─ index.js
```

<br>

- controllers/index.js

```javascript
import UserController from "./users";

// Routers Controllers
export default [UserController];
```

<br>

- index.js

```javascript
import Controllers from "./controllers";

Controllers.forEach((controller) => {
  app.use(controller.path, controller.router);
});
```

- `forEach` : 앞선 controllers/index.js에서 export를 배열형식으로 했으며 `forEach`는 배열의 각 요소에 대해 주어진 함수를 실행한다. `app.use(controller.path, controller.router)` 를 실행시켜준다.
  - 이로써, 각 컨트롤러의 라우터를 앱에 등록해줄 수 있다.
- 각 Controller 에서는 `new UserController();`를 호출하지 않아도 된다. 왜냐하면, `UserController` 클래스를 이미 생성하고 export하고 있기 때문에 다른 파일에서는 해당 클래스의 인스턴스를 생성할 필요가 없다.
- 반복적인 작업을 줄인다. 코드의 가독성이 좋아진다. 에러 찾기가 쉬워진다.

<br>

😭😭😭😭😭

<br>

#### --- Assignment ---

- class, import/export, router, app.use, babel을 사용하여 `/users` 와 `/posts` 에 대한 REST API를 만드시오.

```javascript
import { Router } from "express";

// Router
class PostController {
  router;
  path = "/posts";
  posts = [
    {
      id: 1,
      title: "First post",
      content: "This is first post! so exciting 😆"
    },
    {
      id: 2,
      title: "Second post",
      content: "This is second post! so interesting 🥴"
    },
    {
      id: 3,
      title: "Third post",
      content: "This is third post! so boring 😨"
    }
  ];
  constructor() {
    this.router = Router();
    this.init();
  }

  init() {
    this.router.get("/", this.getPosts.bind(this));
    this.router.get("/post/:id", this.getPost.bind(this));
    this.router.post("/", this.createPost.bind(this));
    this.router.delete("/post/:id", this.deletePost.bind(this));
  }

  getPosts(req, res) {
    res.status(200).json({ posts: this.posts });
  }

  getPost(req, res) {
    const { id } = req.params;
    const post = this.posts.find((post) => post.id === Number(id));
    res.status(200).json({ post });
  }

  createPost(req, res) {
    const { title, content } = req.body;
    this.posts.push({
      id: new Date().getTime(),
      title,
      content
    });
    res.status(201).json({ posts: this.posts });
  }

  deletePost(req, res) {
    const { id } = req.params;
    this.posts = this.posts.filter((post) => post.id !== Number(id));
    res.status(200).json({ posts: this.posts });
  }
}

const postController = new PostController();
export default postController;
```

<br>

```javascript
import PostController from "./posts";
import UserController from "./users";

// Routers Controllers
export default [UserController, PostController];
```

<br>

> ## Error handling

- 에러가 발생했을 때 서버가 다운되지 않고 어떠한 에러인지 클라이언트에 알려주는 역할
- 적절한 메시지와 status를 보내야만 클라이언트에서 혼동이 생기지 않음.

<br>

| 상태 코드                   | 설명                                         |
| --------------------------- | -------------------------------------------- |
| `200` OK                    | 성공적인 요청                                |
| `400` Bad Request           | 클라이언트의 요청이 잘못                     |
| `401` Unauthorized          | 인증이 필요한 요청에 대해 인증되지 않은 접근 |
| `403` Forbidden             | 요청이 서버에 의해 거부                      |
| `404` Not Found             | 요청한 자원이 서버에서 찾을 수 없음          |
| `500` Internal Server Error | 서버에서 처리할 수 없는 오류가 발생했음      |
| `503` Service Unavailable   | 서버가 현재 요청을 처리할 수 없음            |

- `2xx` : 성공적인 응답
- `4xx` : 클라이언트 오류
- `5xx` : 서버 오류

<br>

```javascript
{
  status: 400,
  message: "요청이 잘못됐습니다."
}
```

<br>

```javascript
{
  status: 404,
  message: "유저를 찾을 수 없습니다."
}
```

<br>

```javascript
{
  status: 500,
  message: "서버 에러입니다."
}
```

<br>

### Next

```javascript
getUser(req, res, next) {
  try {
    const { id } = req.params;
    const user = this.users.find((user) => user.id === Number(id));

    if(!user) {
      throw { status: 404, message: "유저를 찾을 수 없습니다." };
    }

    res.status(200).json({ user });
  } catch(err) {
    next(err);
  }
}
```

- `throw`을 만나면 해당 코드를 멈추고 `catch`로 이동하여 `next`를 만나게 된다.
- 반대로 `user`가 있어 에러가 발생하지 않으면 `res.status(200).json({ user })`를 통해 사용자 정보를 반환한다.

<br>

- 간단한 미들웨어 함수

```javascript
function next1(req, res, next) {
  next();
}

function next2(req, res, next) {
  next();
}

function next3(req, res, next) {
  next();
}
```

- 위 코드는 Express에서 미들웨어 함수 체인을 구성하는 기본적인 방법이다.
- 에러가 발생하면 해당 요청은 다음 미들웨어 함수로 전달된다.

<br>

### trycatch

```javascript
  getUsers(req, res, next) {
    try {
      res.status(200).json({ users: this.users });
    } catch (err) {
      next(err);
    }
  }

  getUser(req, res, next) {
    try {
      const { id } = req.params;
      const user = this.users.find((user) => user.id === Number(id));

      if (!user) {
        throw { status: 404, message: "Not Found User" };
      }
      res.status(200).json({ user });
    } catch (err) {
      next(err);
    }
  }

  createUser(req, res, next) {
    try {
      const { name, age } = req.body;
      this.users.push({
        id: new Date().getTime(),
        name,
        age,
      });
      res.status(201).json({ users: this.users });
    } catch (err) {
      next(err);
    }
  }
```

<br>

```javascript
app.use((err, req, res, next) => {
  res
    .status(err.status || 500)
    .json({ message: err.message || "INTERNAL SERVER ERROR" });
});
```

<br>

#### ---Assignment---

```javascript
  getPosts(req, res, next) {
    try {
      res.status(200).json({ posts: this.posts });
    } catch (err) {
      next(err);
    }
  }

  getPost(req, res, next) {
    try {
      const { id } = req.params;
      const post = this.posts.find((post) => post.id === Number(id));

      if (!post) {
        throw { status: 404, message: "Not Found Post" };
      }
      res.status(200).json({ post });
    } catch (err) {
      next(err);
    }
  }

  createPost(req, res, next) {
    try {
      const { title, content } = req.body;

      if (!title || !content) {
        throw { status: 400, message: "Title or content is missing." };
      }

      this.posts.push({
        id: new Date().getTime(),
        title,
        content,
      });

      res.status(201).json({ posts: this.posts });
    } catch (err) {
      next(err);
    }
  }

  deletePost(req, res, next) {
    try {
      const { id } = req.params;

      if (!id) {
        throw { status: 400, message: "Not Found Post" };
      }

      this.posts = this.posts.filter((post) => post.id !== Number(id));
      res.status(200).json({ posts: this.posts });
    } catch (err) {
      next(err);
    }
  }
```
