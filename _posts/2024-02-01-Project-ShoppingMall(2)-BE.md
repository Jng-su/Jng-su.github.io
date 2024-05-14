---
# 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-BE (2)
author: JEONGSUJONG
date: 2024-02-01 01:00:00 +0800
toc: true
pin: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> ## Register Router

- "회원가입" 을 클릭하면 client가 thunkFunction 내에서 비동기 요청을 보냄

```javascript
const response = await axiosInstance.post(`/users/register`, body);
return response.data;
```

- 서버에서 이 비동기 요청을 처리해주어야함.

<br>

- routes/user.js

```javascript
const express = require("express");
const router = express.Router();

router.post("/users/register", (req, res) => {
  // User 데이터를 저장해야함.
});

module.exports = router;
```

<br>

- index.js

```javascript
const UserRouter = require("./routes/user-router");
app.use('/api/v1/users', UserRouter);
```

- api/v1 으로 받으니 FE - axios.js 수정해줘야함

```javascript
baseURL: import.meta.env.PROD ? "" : "http://localhost:5000/api/v1",
```

<br>

- Client 에서 요청하는 req.body는 아래와 같다.

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/b67d156b-2550-4107-9ec1-9f15fa795dd4){: width=100% height=100% .normal}

```javascript
router.post("/users/register", async (req, res, next) => {
  try {
    const user = new User(req.body);
    await user.save();
    return res.sendStatus(200);
  } catch (error) {
    next(error)
  }
});
```

- req.body 안에 존재하는 `email` `image` `name` `password` 를 받아온다.

![https://github.com/JEONGSUJONG/readme-main/assets/142254876/02c49e16-d122-44e1-8a5e-00db24e9d982](https://github.com/JEONGSUJONG/readme-main/assets/142254876/02c49e16-d122-44e1-8a5e-00db24e9d982){: width=100% height=100% .normal}

<br>

> ## Password Bcrypt

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/b09b9e6d-78b0-486e-a762-8e2150ad29de){: width=100% height=100% .normal}

- 위의 DB에서는 password가 노출되어 해커가 db를 열게되면 쉽게 user의 password을 알 수 있게된다.

<br>

### 🧷 How to store Password?

1. 원본 비밀번호 저장 (최악)
2. 비밀번호를 암호화 키 (Encryption Key)와 함께 암호화 (양방향)
    - 어떠한 암호를 이용해서 비밀번호를 암호화 하고 그 암호를 이용하여 복호화 가능.
        - "1234" -> 암호화(알고리즘+암호화 키) -> "qUuFwnAdNnDs"
        - "qUuFwnAdNnDs" -> 복호화 -> "1234"
    - 암호화 키가 노출되면 알고리즘은 대부분 오픈되어있어 위험함.
3. SHA256 해시(Hash)해서 저장(단방향)
    - [https://emn178.github.io/online-tools/sha256](https://emn178.github.io/online-tools/sha256)
        - "1234" -> 해시 -> "qUuFwnAdNnDs"
        - "qUuFwnAdNnDs" -> 복호화 불가능!
    - 단, "1234" 와 같은 비밀번호는 "qUuFwnAdNnDs" 와 같이 암호화 되므로 레인보우 테이블(함호화 전과 후를 갖고있는 테이블)을 비교하여 찾아낼 수 있다.
4. 솔트(salt) + 비밀번호(plain pw) 를 해시로 암호화해서 저장
    - "1234" -> salt + "1234" -> 암호화된 값
    - "1234" -> salt + "1234" -> 다른 암호화된 값
    - bcrypt는 salt를 사용하는 암호화 알고리즘으로 그 값을 DB에 저장하게 된다
    
```terminal
npm install bcryptjs --save
```

<br>

#### 📌 bcryptjs to UserSchema

- `userSchema.pre()` : 스키마를 저장하기 전에 먼저 호출되어 해시화를 진행해준다.

```javascript
const bcrypt = require("bcryptjs");
...
userSchema.pre('save', async function (next) {
  let user = this;    // user의 data 들어감 (email, name ...)
  if (user.isModified('password')) {
    const salt = await bcrypt.genSalt(10);
    const hash = await bcrypt.hash(user.password, salt);
    user.password = hash;
  }
  next();
})
// mogoose 하기 전에 작성해줘야함!
const User = mongoose.model("User", userSchema);
```

![https://github.com/JEONGSUJONG/readme-main/assets/142254876/32c6a1a9-0cc9-4956-b36d-802beda0b619](https://github.com/JEONGSUJONG/readme-main/assets/142254876/32c6a1a9-0cc9-4956-b36d-802beda0b619){: width=100% height=100% .normal}