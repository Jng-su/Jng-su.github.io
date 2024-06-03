---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FULL(3)-MainPage
author: JEONGSUJONG
date: 2024-02-23 00:00:00 +0800
toc: true
pin: false
published: true
categories: [personal-project, barrel]
tags: [project, react, node.js]
# image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> # Main Page

<br>

> ## Front

### 🧷 상품 목록 가져오기

```jsx
const limit = 4; // 한 번에 가져올 Product 갯수를 제한한다.
const [products, setProducts] = useState([]); // 상품 목록을 저장하는 상태 변수
const [skip, setSkip] = useState(0); // 가져올 상품 목록 중 "More" 클릭 시 몇 개를 가져올 지 설정
const [hasMore, setHasMore] = useState(false); // 더 이상 상품이 없으면 true
const [filters, setFilters] = useState({
  // 상품 목록의 각 상품들의 표시되는 값을 저장
  continents: [],
  price: []
});
```

<br>

```jsx
useEffect(() => {
  fetchProducts(skip, limit);
}, []);
// 자바스크립트의 hosting 이기 때문에 코드 실행 전에 함수 선언문을
// 먼저 처리하므로 함수를 선언하기 전에도 함수를 호출할 수 있다.
const fetchProducts = async ({
  skip, // "More" 클릭 시 더 보여지는 상품 수
  limit, // 보여지는 상품 최대 개수
  loadMore = false, // DB에 상품이 더 이상 없으면 true
  fiters = {}, // 상품 정보
  searchTerm = "" // 검색 기능
}) => {
  const params = {
    skip,
    limit,
    filters,
    searchTerm
  };
  try {
    const response = await axiosInstance.get("/products", { params: params });
    setProducts(response.data.products);
  } catch (error) {
    console.error(error);
  }
};
```

- `useEffect` 훅은 초기 렌더링 시에 `fetchProducts` 함수를 호출하여 상품 데이터를 가져온다.
- `fetchProducts` 함수는 비동기로 상품 목록을 서버로 부터 가져온다.
  - 함수 내에서 `params` 객체를 생성한다. (GET요청을 위해)
    - `axiosInstance` 앞서 생성한 `params` 객체를 포함시켜 전송한다.
  - 반환된 상품 목록은 `response.data.products` 배열을 `setProducts` 함수를 사용하여 상태 업데이트를 한다. 😇

<br>

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/5480af84-a1bd-47ad-99cc-7cccc7cdabd0) -->

- 서버에 요청을 보낸 것은 확인할 수 있지만 아직 서버의 요청을 받아줄 Route를 만들지 않았다. 🤫

<br>

#### 📌 더보기 설정

```jsx
{
  hasMore && (
    <div>
      <button>More</button>
    </div>
  );
}
```

- `hasMore` 이 `true` 가 되어야 `"More"` 이 보여지게 된다.

<br>

### 🧷 Image

[https://www.npmjs.com/package/react-responsive-carousel](https://www.npmjs.com/package/react-responsive-carousel)

<br>

```jsx
import "react-responsive-carousel/lib/styles/carousel.min.css"; // requires a loader
import { Carousel } from 'react-responsive-carousel';
```

```jsx
const ImageSlider = ({ images }) => {
  return (
    <Carousel autoPlay showThumbs={false} infiniteLoop>
      {images.map((image) => (
        <div key={image}>
          <img
            src={`${import.meta.env.VITE_SERVER_URL}/${image}`}
            alt={image}
            className="w-full h-[120px]"
          />
        </div>
      ))}
    </Carousel>
  );
};
```

<br>

#### 📌 

```jsx
import { Link } from "react-router-dom";
import ImageSlider from "../../../components/ImageSlider";

const CardItem = ({ product }) => {
  return (
    <div className="border-[1px] border-gray-300">
      <ImageSlider images={product.images} />
      <Link to={`/product/${product._id}`}>
        <p>{product.title}</p>
        <p>{product.continents}</p>
        <p className="p-1 text-xs text-gray-500">{product.price}원</p>
      </Link>
    </div>
  );
};
```

<br>

> ## Back

### 🧷 상품 받아오기 Router

```jsx
ProductRouter.get("/", async (req, res, next) => {
  try {
    const products = await Product.find().populate('writer');
    return res.status(200).json({
      products
    })
  } catch (error) {
    next(error)
  }
});
```

- `ProductRouter.get("/")` : HTTP GET 메서드를 통해 `"/products"` 엔드포인트에 대한 라우터를 설정.
- `await Product.find().populate('writer')` : 상품을 DB에서 조회
  - `populate` 는 `writer` 정보를 조회하기 위함. 이는 문서 간에 관계를 설정할 때 한 문서가 다른 문서를 참조할 때 사용. (주로 Id값을 사용)❗
- 참고로 `find` 메서드 외에도 다양한 메서드가 존재
  - `findById()` : 주어진 ID에 해당하는 문서를 반환
  - `findByIdAndUpdate()` : 주어진 ID에 해당하는 문서를 업데이트하고 업데이트된 문서를 반환
  - `findByIdAndDelete()` : 주어진 ID에 해당하는 문서를 삭제하고 삭제된 문서를 반환

<br>

<!-- ![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/d9e45988-8c95-441b-8097-c352285b6ee6) -->

- 클라이언트에서 받은 GET 요청으로 상품들이 정상적으로 나오는 것을 볼 수 있다.
