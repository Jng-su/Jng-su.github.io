---
title: 🍃 SpringBoot - Init Project
date: 2024-07-19 00:00:00 +0800
toc: true
pin: false
published: true
categories: [SERVER, SPRINGBOOT]
tags: [java, springboot]
image: https://github.com/user-attachments/assets/a2dc1a88-2c9b-4d86-92da-1f37f30378f9
---

<br>

---

<br>

🌏 자바공화국에서 살아남기 ch.1

> ## Project Setting

- 개발 환경 세팅
  - [Java SE 22 Archive Downloads](https://www.oracle.com/java/technologies/downloads/#jdk22-windows)
  - [IntelliJ IDE Downloads](https://www.jetbrains.com/ko-kr/idea/download/?section=windows) * Community Version *

- 프로젝트 환경설정
  - [Spring Boot Starter](https://start.spring.io/)

![image](https://github.com/user-attachments/assets/e89239b3-abdf-4261-bce6-d7036aa9d911)

1. 환경변수에 설치된 Java version과 일치해야함!!
2. Generate Project
3. 압축 해제 후 IntelliJ 에서 Open Project
4. `build.gradle` 선택

> 꼭 본인 Java version 확인 후 build.gradle 실행
{: .prompt-warning }

```gradle
java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(22)
	}
}
```

<br>

IntelliJ 실행 후 localhost:8080 접속

![image](https://github.com/user-attachments/assets/fce0f2e8-cdb1-4d02-85c7-1d809426419b)

성공! 😃 ... 환경 설정하느라 오전 날림

아 Setting에서 Gradle 옵션에 Build run, Run tests using을 IntelliJ로 바꾸기!

<br>

만약에 clone 후 다른 로컬에서 서버에서 띄우고 싶다면 `gradle build` 후 실행

gradle 없으면 다운로드해야함

<br>

> ## View Setting

### 📟 `src/main/resources/static/hello-static.html`

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello Spring</title>
    <meta http-equiv="Content-Type" content="text/html" charset="UTF-8" />
</head>
<body>
Hello Spring
</body>
</html>
```

![image](https://github.com/user-attachments/assets/7fe2061d-22e3-4177-8e62-6e622d0f6ab2)

- 정적 컨텐츠 : 뒤에 배울 Controller에 `hello-static`이 없다면 resources내의 `hello-static.html`을 찾고 웹 브라우저에 보냄

<br>

### 📟 `src/main/java/helloGroup.hello_spring/controller/HelloController`

1. `helloGroup.hello_spring` 폴더 내 package생성 
2. package 명은 `controller`로 지정
3. java class를 생성 `HelloController`

```java
package helloGroup.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "hello spring");
        return "hello";
    }
}
```


### 📟 `src/main/resources/templates/hello.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello Spring</title>
    <meta charset="UTF-8">
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}">안녕하세요. 손님</p>
</body>
</html>
```

![image](https://github.com/user-attachments/assets/e3ab38b8-eba2-4636-97e5-25db31718cf7){: width=100% height=100% .normal}

- Controller에서 return 값으로 문자를 반환하면 **viewResolver**가 `resources/templates/` 해당 문자열의 html 파일을 찾아 화면에 보여줌 (`resources/templates/{ViewName}.html`)
- `${data}`에는 hello controller에 정의된 key값(data)을 바꿔줌 

<br>

![image](https://github.com/user-attachments/assets/793bea46-835a-445c-b760-4b6ebdced264)

<br>

> ## build and excute

<br>

1. 해당 프로젝트로 이동
```shell
cd [your-awesome-project]
```
1. gradle.build 실행
```shell
./gradle.bat build
```
1. build 된거 확인
```shell
cd build
cd libs
ls
```
1. jar 파일 실행
```shell
java -jar hello-spring-0.0.1-SNAPSHOT.jar
```

위의 `jar`파일은 서버 배포시 필요함

<br>

> ## MVC (Model View Controller)


### 📟 `Controller`

```java
@Controller
public class HelloController {

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

### 📟 `View`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello Spring</title>
    <meta charset="UTF-8">
</head>
<body>
<p th:text="'hello. ' + ${name}"></p>
</body>
</html>
```

![image](https://github.com/user-attachments/assets/cad25e70-56c8-4e9b-b651-a6db5e3e9ddb)

<br>

> ## API

<br>

- String

```java
    @GetMapping("hello-string")
    @ResponseBody   // HTTP 응답 본문으로 직접 변환
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
```

![image](https://github.com/user-attachments/assets/ac17d8c9-c299-46da-b29f-8d9b03b35315){: width=100% height=100% .normal}

<br>

- Object

```java
    @GetMapping("/hello-api")
    @ResponseBody
    public HelloResponse helloApi(@RequestParam("name") String name) {
        HelloResponse response = new HelloResponse();
        response.setName(name);
        return response;
    }

    static class HelloResponse {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```

![image](https://github.com/user-attachments/assets/cd8c01d2-2c28-48e7-9636-b4eab5dfbd22){: width=100% height=100% .normal}

<br>

![image](https://github.com/user-attachments/assets/07006887-81ad-4945-8872-3a28715e422f)

- `@ResponseBody` 는 ViewResolver 대신에 HttpMessageConverter에게 전달되어 문자 내용을 직접 반환
- 문자처리 : StringHttpMessageConverter
- 객체처리 : MappingJackson2HttpMessageConverter


참고 : IntelliJ에서 get, set 자동완성은 `alt + insert` 누른 뒤 `Getter and Setter` 누르면 됨


<br>

```java
package helloGroup.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "hello spring");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }

    @GetMapping("/hello-api")
    @ResponseBody
    public HelloResponse helloApi(@RequestParam("name") String name, @RequestParam("age") int age) {
        HelloResponse response = new HelloResponse();
        response.setName(name);
        response.setAge(age);
        return response;
    }

    static class HelloResponse {
        private String name;
        private int age;

        //
        public String getName() {
            return name;
        }
        public int getAge() {
            return age;
        }

        //
        public void setName(String name) {
            this.name = name;
        }
        public void setAge(int age) {
            this.age = age;
        }
    }
}
```