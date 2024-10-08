---
title: 👀 Git | Commit Convention, Issue label/template
date: 2024-01-22 00:00:00 +0800
toc: true
pin: true
published: true
categories: [team-project, github]
tags: [github]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/9493772d-5a56-497b-90a3-87a8c7cfe761
---

<br>

> ## Commit Convention

#### create a template file

```shell
touch .gitcommit.txt
```

#### gitcommit.txt

```
############ Input Field ############
# Title: Description

# Body

######################################

# ✨ Feat : 새로운 기능 추가
# 🚧 Progress : 작업 진행 중인 코드
# 🎯 Fix : 코드 수정
# 🐛 Bug : 버그 수정
# 🎨 Design : CSS 등 사용자 UI 디자인 변경
# 💄 Style : 코드 포맷 변경, 비즈니스 로직 변경 X
# ♻️ Refactor : 프로덕션 코드 리팩토링
# 💡 Comment : 필요한 주석 추가 및 변경
# 📋 Docs : 문서 수정 (README 포함)
# ✅ Test : 테스트 추가, 리팩토링
# 🔖 Chore : 빌드 태스크 업데이트, 패키지 매니저 수정
# 📝 Rename : 파일 혹은 폴더명 수정
# 🔥 Remove : 사용하지 않는 파일 혹은 폴더 삭제
# 📌 Init : 초기 생성
# 🚑 !BREAKING CHANGE : 큰 API 혹은 로직 변경
# 🔔 Merge Request : Merge Request 생성

# 🚨 CAUTION
# - 제목 첫 글자는 대문자로 작성합니다.
# - 제목은 명령문으로 작성합니다.
# - 제목 끝에 마침표(.) 금지는 필수입니다.
# - 제목과 본문을 한 줄 띄워 분리합니다.
# - 본문은 “무엇을”, “왜”를 설명합니다.
# - 본문에 여러 줄 메시지를 작성할 땐 "-"로 구분합니다.
```

#### Set the Commit Template in Git

```shell
git config commit.template .gitcommit.txt

or

git config --global commit.template .gitcommit.txt
```

#### Excute

```shell
git add .
git commit
```

<br>

> ## Issue

### Label

원하는 경로에 `label.json` 파일을 생성 : 개인적으로 필요한 라벨이 프로젝트 별로 다르기에 진행중인 프로젝트 폴더에 넣는게 좋을 듯

```json
[
  {
    "name": "⚙ Setting",
    "color": "e3dede",
    "description": "개발 환경 세팅"
  },
  {
    "name": "✨ Feature",
    "color": "a2eeef",
    "description": "기능 개발"
  },
  {
    "name": "🌏 Deploy",
    "color": "C2E0C6",
    "description": "배포 관련"
  },
  {
    "name": "🎨 Html&css",
    "color": "FEF2C0",
    "description": "마크업 & 스타일링"
  },
  {
    "name": "🐞 BugFix",
    "color": "d73a4a",
    "description": "Something isn't working"
  },
  {
    "name": "💻 CrossBrowsing",
    "color": "C5DEF5",
    "description": "브라우저 호환성"
  },
  {
    "name": "📃 Docs",
    "color": "1D76DB",
    "description": "문서 작성 및 수정 (README.md 등)"
  },
  {
    "name": "📬 API",
    "color": "D4C5F9",
    "description": "서버 API 통신"
  },
  {
    "name": "🔨 Refactor",
    "color": "f29a4e",
    "description": "코드 리팩토링"
  },
  {
    "name": "🙋‍♂️ Question",
    "color": "9ED447",
    "description": "Further information is requested"
  },
  {
    "name": "🥰 Accessibility",
    "color": "facfcf",
    "description": "웹접근성 관련"
  },
  {
    "name": "✅ Test",
    "color": "ccffc4",
    "description": "test 관련(storybook, jest...)"
  }
]
```

깃허브 AccessToken 발급 후 github-label-sync 설치
```shell
sudo npm install -g github-label-sync
```
label 적용
```shell
github-label-sync --access-token [yourtoken] --labels ./labels.json [계정이름/레포명]
```

<br>


### Template

프로젝트 레포지토리 접속 후 "Setting"의 "General"에서 스크롤 내리면 아래와 "Set up template" 클릭
![img](https://github.com/user-attachments/assets/8203ea49-e719-4629-aade-646e96ad2352)
"Add template" 토글 클릭 후 "custom template" "Preview Edit" 클릭 후 연필 모양 (✎) 클릭
![image](https://github.com/user-attachments/assets/f370e49e-6c72-4a8f-8d22-ce9a4325dcfa)

<br>

#### 🚀 Pull Request

```
## 연관된 이슈

> ex) #이슈번호, #이슈번호

## 작업 내용

> 이번 PR에서 작업한 내용을 간략히 설명해주세요(이미지 첨부 가능)

### 스크린샷 (선택)

## 리뷰 요구사항(선택)

> 리뷰어가 특별히 봐주었으면 하는 부분이 있다면 작성해주세요
>
> ex) 메서드 XXX의 이름을 더 잘 짓고 싶은데 혹시 좋은 명칭이 있을까요?
```

#### 🐞 Bug Report

```
## 버그 설명

> 발견한 버그를 간략히 설명해주세요.

## 재현 단계

1. 
2. 
3. 

## 예상 결과

> 기대했던 결과를 설명해주세요.

## 실제 결과

> 실제로 발생한 결과를 설명해주세요.
```

#### ✨ Feature Request

```
## 기능 설명

> 요청하고 싶은 기능을 간략히 설명해주세요.

## 기대 효과

> 이 기능이 사용자나 프로젝트에 어떤 이점을 제공하는지 설명해주세요.
```