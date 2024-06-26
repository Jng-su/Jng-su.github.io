---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: backend(6)-AWS
author: JEONGSUJONG
date: 2024-03-18 00:00:00 +0800
toc: true
pin: false
published: false
categories: [tech, node.js]
tags: [aws]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/580ff546-c5d4-4502-a0c3-489b13a49870
---

<br>

> ## AWS

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/1476c5b3-ac2a-4dfd-aceb-48a7913e1d77) -->

- cloud service : 본인 컴퓨터가 아닌 다른 곳에서 서버를 가동 시킬 수 있다.
  - AWS : 저렴한 비용, 배포 속도 개선, 유지 비용 개선

<br>

### 서버 배포 방식

<!-- ![image](https://github.com/JEONGSUJONG/github-mainpage/assets/142254876/267be03e-e8e3-4980-b906-b778ca4b050c) -->

- 빌드 : 실행 가능한 상태로 만들기
- 배포 : 실행 가능한 코드로 만들어진 코드 업로드

<br>

## PM2

- 코드를 백그라운드에서 코드를 실행할 수 있도록 도와주는 툴
- 코드를 CPU 개수 만큼 실행할 수 있다.

| 명령어                       | 설명                                                                 |
| ---------------------------- | -------------------------------------------------------------------- |
| `npm install -g pm2`         | pm2 설치                                                             |
| `pm2 start index.js --watch` | index.js를 pm2로 실행 (`--watch`는 nodemon 처럼 코드 변경 시 재실행) |
| `pm2 ls`                     | 실행 중인 pm2 리스트 조회                                            |
| `pm2 stop [id]`              | 특정 프로세스 중단                                                   |
| `pm2 delete [id]`            | 특정 프로세스 삭제                                                   |
| `pm2 kill`                   | 실행 중인 pm2 종료                                                   |

<br>

`pm2 start src/index.js`
