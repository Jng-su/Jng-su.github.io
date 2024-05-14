---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: ReactJS로 영화 웹 만들기 (07)
author: JEONGSUJONG
date: 2024-01-25 00:00:00 +0800
toc: true
pin: false
categories: [lecture, ReactJS로 영화 웹 만들기]
tags: [react]
image: https://github.com/JEONGSUJONG/Readme_main/assets/142254876/7dd6d929-f416-492a-b255-f17f99c5b5a7
---

<br>

> ## MOVIE APP

- MOVIE_API : [https://yts.mx/api](https://yts.mx/api)
  - [https://yts.mx/api/v2/list_movies.json?sort_by=download_count](https://yts.mx/api/v2/list_movies.json?sort_by=download_count)
    - 오랜된 영화가 많아서 `download_count` Query를 적용한 API를 가져왔다.
  - [https://yts.mx/api/v2/movie_details.json?movie_id](https://yts.mx/api/v2/movie_details.json?movie_id=)
    - 영화 상세 페이지로 이동하기 위한 API

<br>

### 🧷 Using `then` Method

```jsx
useEffect(() => {
  fetch(`https://yts.mx/api/v2/list_movies.json?sort_by=download_count`)
    .then((Response) => Response.json())
    .then((json) => {
      setMovies(json.data.movies);
      setLoding(false);
    });
}, []);
```

- `fetch` 를 사용하고 `then` 메서드를 이용하여 비동기적으로 데이터 처리.

<br>

### 🧷 Using `async/await` Method

```jsx
const getMovies = async () => {
  try {
    const json = await (
      await fetch(
        `https://yts.mx/api/v2/list_movies.json?sort_by=download_count`
      )
    ).json();
    setMovies(json.data.movies);
    setLoding(false);
  } catch (error) {
    console.error("Error fetcing movies: ", error);
  }
};
```

- `await` 를 사용하여 비동기적으로 데이터 처리

<br>

#### 📌 Summary

- `then` 메서드를 사용한 코드는 콜백 형태로 비동기 코드를 작성
- `async/await` 를 사용한 코드는 비동기 코드를 작성하지만 코드의 구조와 동기적으로 보이도록 해준다. 즉 가독성이 좋다.
- 나는 앞으로 `async/await` 를 사용하여 할 것이다.

```jsx
return (
  <div>
    {loading ? (
      <h1>Loading ...</h1>
    ) : (
      <div>
        {movies.map((movie) => (
          <div key={movie.id}>
            <img src={movie.medium_cover_image} alt={movie.title} />
            <h2>{movie.title}</h2>
            <h3>{movie.year}</h3>
            <p>{movie.summary}</p>
            {movie.hasOwnProperty("genres") ? (
              <div>
                <h4>[GENRES]</h4>
                <p>{movie.genres.join(", ")}</p>
              </div>
            ) : null}
            <hr />
          </div>
        ))}
      </div>
    )}
  </div>
);
```

- `Loading` 상태가 true면 `<h1>Loding ...</h1>` 를 렌더링한다.
- 데이터가 로딩되면 `movies.map` 을 사용하여 각각의 영화 데이터를 순회한다.
- 각 영화는 `movie.id` 로 구분되며 그 안에는 제목(title), 요약(summary), 장르(genres) 가 포함된다.
  - 장르(genres) 또한 `map` 함수를 사용하여 `<li>`로 나열한다.

<br>

> ## Export Movis.js

- Moive.js

```jsx
import PropTypes from "prop-types";

function Movie({ coverImg, title, summary, genres }) {
  return (
    <div>
      <img src={coverImg} />
      <h2>{title}</h2>
      <p>{summary}</p>
      {genres ? (
        <div>
          <p>{genres.join(", ")}</p>
        </div>
      ) : null}
    </div>
  );
}

Movie.propTypes = {
  coverImg: PropTypes.string.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired
};

export default Movie;
```

- `Movie` 컴포넌트는 영화 정보를 표시하기 위한 함수형 컴포넌트를 정의한다.
- `coverImg, title, summary, genres` 를 props 로 받아와 해당 정보를 화면에 렌더링한다.
- `genres` 가 존재하면 장르들을 콤마(,)로 구분하여 표시
  - `join()` 은 JavaScript 배열의 메서드로 배열의 모든 요소를 하나의 문자열로 결합한다.
- `arrayOf()` 는 React PropTypes에서 제공하는 메서드로 특정 타입으로 이루어진 배열을 나타낸다.

<br>

```jsx
import Movie from "./Movie.js";

...

return (
  ...
  <div>
    {movies.map((movie) => (
      <Movie
        key={movie.id}
        coverImg={movie.medium_cover_image}
        title={movie.title}
        summary={movie.summary}
        genres={movie.genres}
      />
    ))}
  </div>
);
```

<br>

> ## React Router

- npm install react-router-dom (currentVersion : 6)
- React 애플리케이션에서 페이지 네비게이션을 관리하는 라이브러리이다.
  - 페이지 간의 이동 및 라우팅을 효과적으로 관리하기 위해 사용된다.
- 여러 페이지를 만들고, 각 페이지에 대응하는 컴포넌트를 연결할 수 있다.

<br>

### 🧷 Folder Structure

```
├─ src
│   ├─ component
│   │   ├─ Movie.js
│   ├─ routes
│   │   ├─ Detail.js
│   │   ├─ Home.js
├─ App.js
├─ index.js
├─ style.css
```

- `/routes` : 애플리케이션의 각 페이지에 대응하는 React 컴포넌트를 저장한다.
  - `./Detail.js` : 영화 상세 페이지로 이동한다.
  - `./Home.js` : 홈 페이지에 대응하는 React 컴포넌트를 정의한다.
- `App.js` : 애플리케이션의 메인 컴포넌트를 정의한다.
  - 여기서 `react-router-dom` 을 사용하여 여러 페이지 간의 라우팅을 설정한다.
- `index.js` : React 애플리케이션 진입점(entry point)으로 `ReactDOM.render`를 사용하여 실제 화면에 React 앱을 렌더링한다.

<br>

#### 📌 How to use React router?

- App.js

```jsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./routes/Home";
import Detail from "./routes/Detail";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/movie" element={<Detail />} />
        <Route path="/" element={<Home />} />
      </Routes>
    </Router>
  );
}

export default App;
```

- `react-router-dom` 에서 필요한 컴포넌트를 가져온다.
  - `import { BrowserRouter as Router, Routes, Route,}`
    - `Router` 컴포넌트를 생성하고 그 안에서 다양한 경로에 대응하는 라우트를 정의한다.
    - `Route` 컴포넌트를 사용하여 경로와 해당 컴포넌트를 지정 (`<Route path="/movie" element={<Detail />} />`)
- `/` 는 홈 페이지, `/movie` 는 영화 상세 페이지로 렌더링된다.

<br>

#### 📌 Link to

- 영화 coverImg 를 클릭하면 해당 영화의 상세페이지(Detail)로 이동하길 원한다..!
- `<a href = "...">` 를 사용하면 되지만 모든 페이지가 렌더링 되기때문에 React사용하는 이유가 없음
- React에서는 `<Link to = "...">` 를 사용한다.
- `import { Link } from "react-router-dom"`

<br>

- Movie.js

```jsx
import { Link } from "react-router-dom";

...
<Link to="/movie">
  <img src={coverImg} />
</Link>
```

- page가 reload 되지 않고 이동하게 된다..!

<br>

#### 📌 id

```jsx
<Route path="/movie/:id" element={<Detail />} />
```

- `:id` 가 포함된 경우, React Router에서는 해당 위치에 오는 동적인 값을 id 로 간주
  - `:id` 에는 다양한 값이 들어올 수 있으며, 이 값을 활용하여 동적인 페이지를 렌더링할 수 있다.
- `/movie/:id` 는 `/movie/` 다음에 오는 값이 `id` 에 해당한다는 의미

<br>

```jsx
<Link to={`/movie/${id}`}>
  <img src={coverImg} />
</Link>
```

- coverImg 를 클릭하면 Link to 를 타서 해당 id가 포함된 url로 이동하게 된다.
- 근데 React Router한테 해당 id 값이 뭔지 알아야한다.

  - `import { useParams } from "react-router-dom";` : url에 있는 값을 반환해주는 함수.

<br>

- Detail.js

```jsx
import { useParams } from "react-router-dom";

...

const { id } = useParams();
const [loading, setLoding] = useState(true);
const [movie, setMovie] = useState(null);

const getMovie = async () => {
  const json = await (
    await fetch(`https://yts.mx/api/v2/movie_details.json?movie_id=${id}`)
  ).json();
  setMovie(json.data.movie);
  setLoding(false);
};
useEffect(() => {
  getMovie();
});
```

- `const { id } = useParams();` : React Router의 훅 중 하나로, 현재 URL에서 추출한 동적인 값을 객체로 반환한다.
- `getMovie` 함수는 `fetch` 를 사용하여 영화 정보를 가져오는 비동기 함수이다.
    - 앞서 추출한 `id` 값을 넣어준다.
    - `json.data.movie` 해당 데이터의 속성을 추출하여 `setMoive` 함수를 사용해 `movie` 상태를 업데이트한다.