---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (02)
author: JEONGSUJONG
date: 2024-01-17 00:00:00 +0800
toc: true
pin: false
published: false
categories: [lecture, ReactJS로 영화 웹 만들기]
tags: [react]
image: https://github.com/JEONGSUJONG/Readme_main/assets/142254876/7dd6d929-f416-492a-b255-f17f99c5b5a7
---

<br>

> ## Time Converter

```jsx
const [minutes, setMinutes] = React.useState();
function onChange(event) {
  setMinutes(event.target.value);
}
```

- `onChange` 함수는 event 를 감지하고 `setMinutes` 를 사용해 감지된 event를 현재 `mintues` 에 입력한다.

<br>

```jsx
<div>
  <label>Minutes</label>
  <input
    value={minutes}
    id="minutes"
    placeholder="Minutes"
    type="number"
    onChange={onChange}
  />
</div>
```

- `minutes` 값을 input의 value에 넣는다.
- `onChange` 함수를 사용하여 input 값이 변경될 때마다 `onChange` 함수를 호출한다.
  - `onChange` : input에서 포커스가 벗어났을 때 input에 입력된 값이 이전과 다르면 `onChange` 이벤트가 발생함.

<br>
 
> ## Flipped

```jsx
<div>
  <label>Hours</label>
  <input
    value={Math.round(minutes / 60)}
    id="hours"
    placeholder="Hours"
    type="number"
    disabled
  />
</div>
```

- `disabled` : 기본적으로 `true` 이고 input를 작성할 수 없다.

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/4b3502fb-e563-45fc-8fc1-1004e73564ae){: width="400" height="250" .normal}

<br>

- flip은 "분을 시간으로" , "시간을 분으로" 하기 위해 disabled 를 조작할 수 있어야한다.
- 현재 `Hours` 가 `disabled=true` 이므로 Flip을 위한 함수를 작성한다.

```jsx
const [flipped, setFlipped] = React.useState(false);
const onFlip = () => setFlipped((current) => !current);
```

- init 값을 `false` 로 해주고 Click event 가 발생하면 `true` 로 바꿔준다.

```jsx
<button onClick={onFlip}>Flip</button>
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/caa57b57-d217-4340-bc91-76da3c956964){: width=100% height=100% .normal}

<br>

```jsx
<div>
    <label>Minutes</label>
    <input
        value={flipped ? amount * 60 : amount}
        id="minutes"
        placeholder="Minutes"
        type="number"
        onChange={onChange}
        disabled={flipped} />
</div>

<div>
    <label>Hours</label>
    <input
        value={flipped ? amount : Math.round(amount / 60)}
        id="hours"
        placeholder="Hours"
        type="number"
        onChange={onChange}
        disabled={!flipped} />
</div>
```

- current `flipped` 값은 false 이므로 `disabled={!flipped}` == `true`
- `setFlipped((current) => !current);` : 현재 flipped 값을 바꿔준다.
- `value={flipped ? amount * 60 : amount}` : flipped 가 `true` 이면 `amount * 60` 이 되므로 `amount` 즉, Hours 의 value 값이 `amount` 가 된다.
  - `flipped` : `true` -> `hours` / `value` : 1 -> `Minutes` / `value` : 1 \* 60 = 60 이 된다.
- `mintues` -> `amount` : "분에서 시간으로" 바꾸면 "분"에 입력한 값이 "시간" 으로 가기 위해 변수명 변경

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/af2d1a61-76f4-4044-88a7-662ff7e1f025){: width="400" height="250" .normal}

<br>

> ## Select Component

```jsx
function TimeConverter() { ... }
function DistanceConverter() { ... }

function App() {
    const [index, setIndex] = React.useState(0);
    function onSelect() {
        setIndex(event.target.value)
    }
    return (
        <div>
            <h2>🎯 Super Converter</h2>
            <select value={index} onChange={onSelect}>
                <option value="Text">Select Your Units!</option>
                <option value="0">TimeConverter</option>
                <option value="1">DistanceConverter</option>
            </select>
        </div>
    );
};
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/6ccde7bf-b592-4b12-868c-b9df311951a0){: width="400" height="250" .normal}

- `<option value = "0">` : `App` 함수는 `select` 컴포넌트를 이용하여 선택된(?) value 값에 해당하는 `option` 을 선택할 수 있다.
  - `select value = {index}` 는 index 이벤트를 `onChange` 로 감지하고 `setIndex` 를 통해 현재 index 값을 알 수 있다.

```jsx
<div>
  ...
  {index == "Text" ? <h2>Please Select Your Units!</h2> : null}
  {index == "0" ? <TimeConverter /> : null}
  {index == "1" ? <DistanceConverter /> : null}
</div>
```

- javascript 를 사용하기 위해 `{}` 로 감싸고 작성한다.
- index 의 값에 따라 실행될 함수를 선택할 수 있다.
