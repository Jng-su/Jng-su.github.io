---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (05)
author: JEONGSUJONG
date: 2024-01-23 00:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, ott]
tags: [react]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/a7d1fbfa-583b-40c9-b3be-1fc0e42ba1e0
---

<br>

> ## To do list

```jsx
<form onSubmit={handleSubmit}>
  <input
    onChange={handleToDo}
    value={toDo} 
    type="text"
    placeholder="Write Your to do"
  />
  <button>Add To Do</button>
</form>
```

- 사용자가 텍스트를 입력하면 `onChange` 이벤트 핸들러인 `handleToDo` 함수가 호출되어 입력한 값을 상태 변수 `toDo` 에 업데이트한다.
- `handleToDo` : 입력 폼의 값이 변경될 때 호출되며, 현재 입력된 값을 toDo 상태로 업데이트합니다.

<br>

```jsx
const [toDo, setToDo] = useState("");
const [toDos, setToDos] = useState([]);
const handleToDo = (event) => setToDo(event.target.value);
const handleSubmit = (event) => {
  event.preventDefault();
  if (toDo === "") {
    alert("Write Your To Do 🙁");
    return;
  }
  setToDos((currentArray) => [toDo, ...currentArray])
  setToDo("");
}
```

- `handleSubmit` : 폼이 제출될 때 호출되며, 먼저 입력된 값이 비어 있는지 확인한 후, 비어 있지 않으면 현재 입력된 할 일을 `toDos` 배열에 추가하고 `toDo` 를 초기화
    - `setToDos((currentArray) => [toDo, ...currentArray])` : submit 된 상태 값인 `toDo` 를 `currentArray` 에 넣어준다.
    - `setToDo("")` : 입력하는 용도인 setToDo를 입력 후 빈 문자열을 보내준다.

<br>


### 🧷 li tag

- `map` 함수

```javascript
const food = ["apple", "tomato", "pumpkin"];
food.map((item) => item.toUpperCase());
```

```
(3) ['APPLE', 'TOMATO', 'PUMPKIN']
```

- 배열의 각 요소에 대해 제공된 함수를 호출하고, 그 결과로 새로운 배열을 생성한다.
- `map` 함수를 사용하여 각각의 component들을 return 하고 li 태그로 감싸면 된다...!

<br>

```jsx
<ul>
  {toDos.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/dcccad8c-9bf2-45fe-a5bc-66d88157e46a){: width=100% height=100% .normal} -->

- 같은 component의 list를 render 할 경우 `key` 라는 prop을 넣어주어야한다.
  - `key` 는 고유의 값 이어야 한다.
