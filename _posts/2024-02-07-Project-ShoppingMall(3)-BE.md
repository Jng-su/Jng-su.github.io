---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-BE (3)
author: JEONGSUJONG
date: 2024-02-07 00:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## 인증 절차가 필요한 이유

- 만약 client와 server 간의 첫 번째 요청에서 자신이 "123"이라고 말해도 그 후 서버에게 다시 물어보면 서버는 내가 누군지 모른다.
- HTTP 는 stateless 하기 때문이다.
  - 상태 비저장 프로토콜은 서버가 여러 요청 기간 동안 각 사용자에 대한 정보나 상태를 유지할 필요가 없다.
    - 성능(performance) 저하 문제 : 각 요청에 대한 연결을 재설정하는 데 소요되는 시간/대역폭을 최소화하기 위한 것
    - 만약, 요청 하나하나에 사용자의 status 와 info를 포함하면 server는 누군지는 알지만 많은 트래픽으로 성능 저하의 원인이 된다.


<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/c10dbbd4-628c-4ba6-9d47-bff326480e2e){: width=100% height=100% .normal} -->


- (2) Token 안에 유저의 정보를 포함하는 토큰을 생성한다.
- (3) 응답을 보낼 때 HTTP header 에 Token과 같이 보낸다.
- (5) 요청을 보낼 때 역시 Token을 같이 보내준다.
- (6) server에서는 Token을 복호화해서 갖고 있는 유저 정보를 알 수 있다. (stateless -> statefull)


> ## JWT

- JWT (JSON Web Token) : 당사자간에 정보를 JSON 개체로 안전하게 전송하기 위한 컴팩트하고 독립적인 방식을 정의하는 개방형 표준 (RFC 7519) 이다.
- 정보를 안전하게 전할 때 혹은 유저의 권한 같은 것을 체크 하기 위해서 사용되는 모듈

<br>

### 🧷 JWT 구조

[https://jwt.io/](https://jwt.io/)

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/007bafd4-83ee-4f80-9d31-50616c53f601){: width=100% height=100% .normal} -->

- Header(red) : Token에 대한 메타 데이터 포함 (타입, 해싱 알고리즘)
- Payload(purple) : 유저 정보(issuer), 만료 기간(expiration time), 주제(subject) 등등 ..
- Verify Signature(blue) : Token이 보낸 사람에 의해 서명되었으며 어떤 식으로도 변경되지 않았는지 확인하는 데 사용되는 서명
    - 서명은 헤더 및 페이로드 세그먼트, 서명 알고리즘, 비밀 / 공개 키를 사용하여 생성됨.


<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/2d4ffbd9-9cf1-4618-823e-36af859ffb46){: width=100% height=100% .normal} -->


1. Admin 유저가 보고자 할 경우 (관리자 권한 페이지)
2. 요청을 보낼 때 Token을 Header에 넣어서 같이 보냄
3. 서버에서는 JWT를 이용하여 Token을 다시 생성한 후 두 개를 비교
    - 서버에서 요청에서 같이 온 Header랑 payload를 가져오고 서버안에 있는   Secret 을 이용하여 Signature 부분을 다시 생성
    - Client에서 온 Headers + Client에서 온 Payload + Server에서 갖고 있는 Secret Text
4. 일치하면 Admin 유저가 원하는 글을 볼 수 있다.


<br>

> ## Login Router 생성

- FE / store / thunkFunction.js

```javascript
const response = await axiosInstance.post(`/users/login`, body);
```

<br>

- 현재 post 요청을 받아올 API 가 필요하다
- users-router.js

```javascript
// Login
UserRouter.post("/login", async (req, res, next) => {
  // req.body : email , password
  try {
    // 존재하는 유저인지 체크

    // 비밀번호가 올바른지 체크 (comparePassword 함수)

    // JWT 토큰생성

    // 토큰 생성 후 유저와 토큰 데이터 응답으로 보내주기
  } catch (error) {
    
  }
})
```

<br>

- comparePassword 함수는 user-shema 에서 정의한다.

```javascript
userSchema.methods.comparePassword = async function (plainPassword) {
  let user = this;

  const isMatch = await bcrypt.compare(plainPassword, user.password);
  return isMatch;
};
```

- 기능은 사용자의 비밀번호 비교를 수행한다.
- plainPassword 를 해시된 비밀번호와 비교하여 일치 여부를 return 한다.

<br>

- users-router.js

```javascript
// Login
UserRouter.post("/login", async (req, res, next) => {
  // req.body : email , password(plainText)
  try {
    // 존재하는 유저인지 체크
    const user = await User.findOne({ email: req.body.email });
    if (!user) {
      return res.status(400).send("Auth failed, email not found");
    }
    // 비밀번호가 올바른지 체크
    const isMatch = await user.comparePassword(req.body.password);
    if (!isMatch) {
      return res.status(400).send("Wrong password");
    }
    const payload = {
      userId: user._id.toHexString(), // Obj Id 이기 때문에 String으로 변환
    }
    // JWT Token 생성
    const accessToken = JWT.sign(payload, process.env.JWT_SECRET, { expiresIn: '1h' });
    // 토큰 생성 후 유저와 토큰 데이터 응답으로 보내주기
    return res.json({ user, accessToken });
  } catch (error) {
    next(error);
  }
})
```

- `userSchema` 에서 받아온 `isMatch` 로 평문과 암호화된 비밀번호를 비교한 값으로 판단한다.
- `payload` 는 토큰에 담기는 정보를 정의하고 여기선userId가 들어가있다.
- `JWT.sign` 메소드는 `payload` 와 Secret Key로 JWT 토큰을 생성한다.

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/373897ee-1158-4393-ac94-c5f137d2194d){: width=100% height=100% .normal} -->

- return res.json({ user, accessToken }) 에서 user 데이터를 반환하면 보안적 측면에서 위험하지 않을까?
    - Client와 Server간의 통신에서 인증 정보를 주고 받을 때 일반적으로 db에 저장된 회원 객체에서 Access Token은 필요한 정보만 추출하여 따로 저장하게 된다.
    - 또한, user 데이터를 반환하면 클라이언트에서 해당 정보를 사용하여 로그인한 사용자의 상태를 지속적으로 유지할 수 있다.
        - 로그인 한 번 하고나면 토큰을 이용하여 인증 정보를 검증할 때마다 모든 인증과정을 반복할 필요가 없이 로그인 상태가 유지된다.


<br>

> ## Auth

- users-router.js

```javascript
// Auth by Token
UserRouter.post("/auth", auth, async (req, res, next) => {
  return res.json({
    id: req.user._id,
    email: req.user.email,
    name: req.user.name,
    role: req.user.role,
    image: req.user.image,
  });
});
```

- "/auth" 엔드 포인트에 대한 Post 요청을 처리한다.
- 요청이 들어오면 auth 미들웨어를 실행하여 토큰의 유효성 검사 후 사용자 인증을 한다.
- JSON 형식으로 반환

<br>

- auth.js : 이 미들웨어는 클라이언트에서 HTTP 요청으로 받아온 Token을 헤더에서 가져와 분석한다.

```javascript
const jwt = require('jsonwebtoken');
const User = require("../models/user-schema");

let auth = async (req, res, next) => {
    // Token을 request headers 에서 가져오기
    const authHeader = req.headers['authorization'];

    // Bearer ---.---.---
    const token = authHeader && authHeader.split(" ")[1];
    if (token === null) return res.sendStatus(401);

    try {
        // Token이 유효한지 확인
        const decode = jwt.verify(token, process.env.JWT_SECRET);
        const user = await User.findOne({ _id: decode.userId });

        if (!user) {
            return res.status(400).send("사용자를 찾을 수 없습니다.");
        }
        req.user = user;
        next();
    } catch (error) {
        next(error);
    }
}

module.exports = auth;
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/c983ed2a-aa0d-4a04-b92d-db6de641d2c3){: width=100% height=100% .normal} -->

- `Bearer ---.---.---` 형식의 토큰을 `" "` 로 구분하여 헤더 중 토큰 부분만 가져온다.
- jwt decode 를 이용하여 유효한지 검사한다.