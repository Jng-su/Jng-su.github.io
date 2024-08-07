---
title: 🍃 SpringBoot [1]
date: 2024-08-01 00:00:00 +0800
toc: true
pin: false
published: true
categories: [SERVER, SPRINGBOOT]
tags: [springboot]
image: https://github.com/user-attachments/assets/a2dc1a88-2c9b-4d86-92da-1f37f30378f9
---

<br>

---

<br>

🌏 자바공화국에서 살아남기 ch.1

Refer to [https://velog.io/@codingrecipe/posts](https://velog.io/@codingrecipe/posts)


> ## Project Setting

- 개발 환경 세팅
  - openJDK 17.0.12
  - IntelliJ IDE Downloads *** Community Version ***
  - MySQL 9.0.1

- 프로젝트 환경설정
  - [Spring Boot Starter](https://start.spring.io/)

![image](https://github.com/user-attachments/assets/c17e8d11-99ed-4502-9640-6234019caadb)

1. 환경변수에 설치된 Java version과 일치해야함!!
2. Generate Project
3. 압축 해제 후 IntelliJ 에서 Open Project
4. `build.gradle` 선택

> 꼭 본인 Java version 확인 후 build.gradle 실행
{: .prompt-warning }

```gradle
java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(17)
	}
}
```

<br>

> ## Project Initialization

### 🖥️ `resources/application.yml`

```yaml
server:
  port: 8080

# database 연동 설정
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/[DB Name]?serverTimezone=Asia/Seoul&characterEncoding=UTF-8
    username: [UserName]
    password: [Password]
  thymeleaf:
    cache: false

  # spring data jpa 설정
  jpa:
    database-platform: org.hibernate.dialect.MySQL8Dialect
    open-in-view: false
    show-sql: true
    hibernate:
      ddl-auto: create  # create, update
```

> ## Project Execute


### 🖥️ `Application.java`

node의 시작점인 `app.js` 혹은 `index.js` 와 같은 루트 모듈임

```java
package com.example.foo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class FooApplication {
    public static void main(String[] args) {
        SpringApplication.run(FooApplication.class, args);
    }
}
```

### 🖥️ `controller/RootController.java`

```java
package com.example.foo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class RootController {

    @GetMapping("/")
    public String index() {
        return "root";
    }
}
```

- `Controller`, `Servier`, `Repository` 는 어노테이션을 붙혀줘야함.
- `@GetMapping` 뒤의 `/`은 URL이라고 생각하면 편함 
- `return` 값에 문자열을 넣어주면 root 템플릿(`root.html`)을 출력해줌.

### 🖥️ `templates/root.html`

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Root</title>
</head>
<body>
    <h1>Hello World</h1>
    <a href="/member/save">SIGN UP</a>
    <a href="/member/login">SIGN IN</a>
    <a href="/member">MEMBER LIST</a>
</body>
</html>
```

- 회원가입, 로그인, 회원리스트

### 🖥️ `controller/MemberController.java`

```java
package com.example.foo.controller;


import com.example.foo.dto.MemberDto;
import com.example.foo.service.MemberService;
import jakarta.servlet.http.HttpSession;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
@RequestMapping("/api/member")
@RequiredArgsConstructor
public class MemberController {
    // 생성자 주입
    private final MemberService memberService;

    @GetMapping("/save")
    public String saveForm() {
        return "save";    // save.html 반환
    }

    @PostMapping("/save")
    public String save(@ModelAttribute MemberDto memberDto) {
        memberService.save(memberDto);
        return "login";    // login.html 반환
    }

    @GetMapping("/login")
    public String loginForm() {
        return "login";    // login.html 반환
    }

    @PostMapping("/login")
    public String login(@ModelAttribute MemberDto memberDto, HttpSession httpSession) {
        MemberDto loginResult = memberService.login(memberDto);
        if (loginResult != null) {
            httpSession.setAttribute("loginEmail", loginResult.getMemberEmail());
            return "main";    // main.html 반환
        } else {
            return "login";    // login.html 반환
        }
    }

    @GetMapping("")
    public String findAll(Model model) {
        List<MemberDto> memberDtoList = memberService.findAllMembers();
        model.addAttribute("memberList", memberDtoList);
        return "list";    // list.html 반환
    }
}
```

- 어노테이션
  - `@RequestMapping` : 템플릿의 URL 제작
  - `@RequiredArgsConstructor` : 객체 생성자 주입을 위한
    - 생성자 왜?  클래스의 의존성을 명확히 하고, 객체의 일관성을 유지하기 위함.
