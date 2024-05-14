---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(6)-DTO
author: JEONGSUJONG
date: 2024-03-29 00:00:00 +0800
toc: true
pin: false
published: false
categories: [backend, Node.js]
tags: [dto]
image: https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/63a46f26-e1ae-489a-a5ce-154f4d4aa987
---

<br>

> ## DTO (Data Transfer Object)

데이터를 서로 다른 부분 간에 옮기는 데 사용되는 객체. 즉, 데이터베이스로부터 데이터를 가져와서 사용자에게 보여줄 때 혹은 사용자가 웹 페이지에 입력한 정보를 데이터베이스에 저장할 때 사용.

<br>

- DAO : 데이터베이스 통신, 데이터베이스에서 데이터를 읽거나 쓰는 역할
- DTO : 계층 간 통신, 데이터베이스에서 가져온 정보를 service 같은 비즈니스 로직에 전달하거나 사용자가 입력한 정보를 데이터베이스에 저장하는 역할
- VO : 값만 사용, 한 번 설정되면 변경되지 않는 값

<br>

### UserDTO

```javascript
export class UserDto {
  id;
  firstName;
  lastName;
  age;

  constructor(user) {
    this.id = user.id;
    this.firstName = user.firstName;
    this.lastName = user.lastName;
    this.age = user.age;
  }

  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

- `constructor(user)` : 주어진 사용자 객체로부터 DTO를 생성하는 생성자
- `getFullName()` : 사용자의 전체 이름을 반환하는 메서드를 정의한다.

<br>

```javascript
const fullNamewithoutDTO = `${targetUser.firstName} ${targetUser.lastName}`;
const user = new UserDTO(targetUser);
res.status(200).json({ fullName: user.getFullName() });
```

- `fullnamewithoutDTO` : 직접적으로 사용자 객체에서 이름과 성을 추출하여 생성한다.
- `fullName: user.getFullName()` : UserDTO 클래스를 사용하여 새로운 DTO 객체를 생성하는 방식은 사용자 데이터를 캡슐화하여 클래스로 따로 정의함으로써 객체 지향적인 설계 원칙을 따른다.
  - 데이터의 구조와 기능을 하나의 단일 객체로 캡슐화하여 코드의 유지 보수성을 높이고 재사용성이 향상된다.

<br>

- 위의 예시를 보았을 경우 DTO을 사용하지 않으면 코드 중복과 만약에 데이터 구조가 변경되면 해당하는 모든 곳을 변경해주어야 한다.
- 특히, 객체 지향 원칙을 위배한다. 데이터의 은닉 원칙을 위배하여 직접적인 데이터 접근으로 데이터의 캡슐화가 깨질 수 있다.
- 반면에 DTO를 사용하면 재사용성과 유지 보수성 객체 지향 원칙을 준수하게 된다.

<br>

> DTO를 사용하지 않으면 코드 중복과 데이터 구조 변경 시 모든 곳을 변경해야 함으로써 유지 보수가 어렵고, 객체 지향 원칙을 위배하여 데이터 캡슐화가 깨질 수 있지만, DTO를 사용하면 재사용성과 유지 보수성을 높일 수 있으며 객체 지향 원칙을 준수할 수 있습니다.

<br>

### CreateUserDTO

```javascript
export class CreateUserDTO {
  firstName;
  lastName;
  age;

  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  getNewUser() {
    return {
      id: new Date().getTime(),
      firstName: this.firstName,
      lastName: this.lastName,
      age: this.age
    };
  }
}
```

<br>

```javascript
  createUser(req, res, next) {
    try {
      const { firstName, lastName, age } = req.body;

      if (!firstName || !lastName) {
        throw { status: 400, message: "성과 이름을 입력해주세요." };
      }

      const user = new CreateUserDTO(firstName, lastName, age);
      const newUser = user.getNewUser();

      this.users.push(newUser);
      res.status(201).json({ users: this.users });
    } catch (err) {
      next(err);
    }
  }
```
