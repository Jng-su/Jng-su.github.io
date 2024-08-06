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

```