---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FULL(1)-Image
author: JEONGSUJONG
date: 2024-02-21 01:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> # Image 업로드

<br>

> ## Front

### 🧷 react-dropzone

`npm install --save react-dropzone`

- 파일 업로드를 구현하기 위한 라이브러리 (BackEnd의 multer와 함께 사용)
- 사용자가 파일을 끌어다 놓거나 클릭하여 기기에서 파일을 선택가능하게 해준다 👍

### 🧷 react-image-gallery

`npm install react-image-gallery`

- 반응형 이미지 갤러리를 생성하는 기능을 제공한다.
- 이미지를 스와이프하여 표시하거나 자동 재생, 썸네일 및 전체 화면 모드와 같은 기능을 제공한다.
- 시각적으로 도움을 많이 줄 듯 🤔

### 🧷 react-responsive-carousel

`npm install react-responsive-carousel`

- react-image-gallery 와 유사한 성격을 가짐. 
- 쇼핑몰의 상품이나 광고와 같은 이미지들이 자동으로 넘어가거나 할 때 사용된다.

<br>

> ## Back

### 🧷 multer

`npm install async multer`

- Node.js 기반의 라이브러리로 파일 업로드를 처리하는 데 사용된다.
- 주로 Express와 함께 사용되며, FE로 부터 받은 이미지, 비디오 등의 파일을 서버에 안전하게 업로드하게 도와준다.

> Express 에서는 파일을 처리하기 위해 내장된 기능이 제한적이므로 multer와 같은 미들웨어가 필요하다.
{: .prompt-warning }

<br>

#### 📌 주요 기능

1. 파일 업로드 처리 : Front 로 부터 받은 전송된 파일을 서버로 안전하게 업로드한다.
2. 파일 유형 및 크기 제한 : 허용되는 파일 유형 및 최대 파일 크기를 설정하여 보안을 강화한다. 😯
3. 파일 저장 경로 : 업로드된 파일이 저장될 경로를 정의하여 파일 시스템을 관리한다.
4. 다중 파일 업로드 : 한 번에 여러 파일을 업로드하고 처리할 수 있다.
5. 파일명 설정 및 중복 처리 : 업로드된 파일의 이름을 설정하고, 중복된 파일이 있는 경우 처리 방법을 정의 할 수 있다.

<br>

### 🧷 cron

`npm install cron`

- 매일 특정 시간에 업로드된 파일을 처리하는 스케줄링을 만들 수 있다. 
- 즉, 스케줄링은 데이터를 정리해주고 디스크 관리를 해주는 중요한 역할이다. 💿
  - `fs` : 파일의 정보를 읽고 파일의 마지막 수정 일자를 확인하고 삭제하게 할 수 있는 Node.js 모듈이다. (따로 설치할 필요 없음.)