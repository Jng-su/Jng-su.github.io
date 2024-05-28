---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (06)
author: JEONGSUJONG
date: 2024-01-24 00:00:00 +0800
toc: true
pin: false
published: true
categories: [lecture, ReactJS로 영화 웹 만들기]
tags: [react]
image: https://github.com/JEONGSUJONG/JEONGSUJONG/assets/168960634/a7d1fbfa-583b-40c9-b3be-1fc0e42ba1e0
---

<br>

> ## Coin Track

- COIN_API : [https://api.coinpaprika.com/v1/tickers](https://api.coinpaprika.com/v1/tickers)

<br>

### 🧷 Fetching Data

- Code Snippet

```jsx
useEffect(() => {
  fetch("https://api.coinpaprika.com/v1/tickers")
    .then((Response) => Response.json())
    .then((json) => console.log(json));
}, []);
```

<br>

- coin api 를 가져오기 위해 React의 useEffect 훅 사용
- fetch 함수를 사용하여 지정된 coin api 엔드 포인트로 GET 요청을 수행
- 비동기적 네트워크 요청 : fetch 함수는 비동기적으로 작동하여 useEffect 훅이 비동기 코드를 처리할 수 있도록 지원
    - 동기 : 작업의 순차적 진행
    - 비동기 : 작업의 독립적 진행
- Promise 반환 : fetch 함수는 네트워크 요청에 대한 Promise 를 반환한다.
    - Promise : pending (대기중) / fulfilled (이행됨) / rejected (거부됨)
        - 비동기 작업이 완료되면 Promise 는 fulfilled 혹은 rejected 상태가 된다.
        - then 을 통해 "이행됨" 상태를 처리, catch 을 통해 "거부됨" 상태를 처리한다.
        - 이 Promise 는 나중에 완료 혹은 실패할 수 있는 비동기 작업을 나타낸다.
- then : Promise가 성공적으로 완료됐을 때 수행할 작업을 정의
    - (첫 번째 then) : response.json() 사용하여 서버 응답을 JSON 형식으로 파싱한다.
    - (두 번째 then) : 파싱된 JSON 데이터를 출력
- 빈 의존성 배열 : [] 빈 배열이 전달되어 해당 useEffect 컴포넌트가 마운트될 때 한 번만 실행되도록 하는 역할을 한다.

<br>


#### 📌 How to show fetching DATA?

<br>

- 앞서 설명한 todolist와 비슷하게 data를 state에 넣어주면됨.

```jsx
const [loading, setLoding] = useState(true);
const [coins, setCoins] = useState([]);
useEffect(() => {
  fetch("https://api.coinpaprika.com/v1/tickers")
    .then((Response) => Response.json())
    .then((json) => {
      setCoins(json);
      setLoding(false);
    });
}, []);
```

```jsx
return (
  <div>
    <h1>The Coins! {loading ? "" : `(${coins.length})`}</h1>
    {loading ? (
      <strong>Loading...</strong>
    ) : (
      <select>
        {coins.map((coin) => (
          <option key={coin.id}>
            {coin.name} ({coin.symbol}) : ${coin.quotes.USD.price} USD
          </option>
        ))}
      </select>
    )}
  </div>
);
```

<!-- ![gif](https://github.com/JEONGSUJONG/readme-main/assets/142254876/010c468b-7486-4d5a-9623-5b3792a66a1d){: width=100% height=100% .normal} -->