---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn)  🌱 (Link) 🧷(#3) 📌(#4) 👀(Recap)
title: PROJECT-ShoppingMall-FULL(2)-Product
author: JEONGSUJONG
date: 2024-02-21 02:00:00 +0800
toc: true
pin: false
published: false
categories: [project, ShoppingMall]
tags: [project, react, node.js]
image: https://github.com/JEONGSUJONG/readme-main/assets/142254876/60a1ef16-879c-4678-b610-29b7e6bd05ba
---

<br>

> # Product Upload

<br>

> ## Front

### 🧷 Product State 설정

```jsx
const [product, setProduct] = useState({
  title: "",
  description: "",
  price: 0,
  continents: 1,
  image: []
});

const handleChange = (event) => {
  const { name, value } = event.target;
  setProduct((prevState) => ({
    ...prevState,
    [name]: value
  }));
};
```

- `useState()` 훅을 사용하여 초기 상태를 설정해준다.
- `handleChange()` 함수는 입력 요소의 변경 이벤트를 처리하고 해당 입력 요소의 `name` (title, description, ... ) 속성에 맞춰 product 상태를 업데이트 한다.
  - Spread 연산자 (`...prevState`) 를 통해 복제한다. 🔁
  - 즉, React 에서는 State를 업데이트 할 때 이전 상태를 복제하고 변경된 내용만 추가된다.
  - `...prevState` 는 이전 상태의 모든 속성이 새로운 상태에 포함되게 되고 다음 업데이트하려는 속성들을 새로운 값으로 설정하여 새로운 State로 만든다. (불변성 유지)

<br>

#### 📌 input handler 추가

```jsx
// title
onChange={handleChange}
value={product.title}

// description
onChange={handleChange}
value={product.description}

// price
onChange={handleChange}
value={product.price}

// continents
onChange={handleChange}
value={product.continents}
```

- 각 입력 요소를 `handleChange` 함수를 통해 `product.이름` 상태를 업데이트 시켜준다

<br>

### 🧷 Submit

```jsx
const userData = useSelector((state) => state.user?.userData);
const navigate = useNavigate();

const handleSubmit = async (event) => {
  event.preventDefault();

  const body = {
    writer: userData.id,
    ...product
  };

  try {
    await axiosInstance.post("/products", body);
    navigate("/");
  } catch (error) {
    console.error(error);
  }
};

// form
onSubmit = { handleSubmit };
```

- Form 이 제출될 때 호출되며, 입력된 데이터를 서버로 전송한다.
- `useSelector` 훅을 사용하여 Redux 스토어에서 사용자의 데이터를 가져온다. (로그인한 유저가 누구인지 확인)

```jsx
await axiosInstance.post("/products", body);
navigate("/");
```

- axios 를 사용하여 서버에 POST 요청을 보내게 되고 해당 요청이 완료될 때 까지 기다린다 (await)
- 그 후에 `navigate("/")` 가 실행된다. React Router의 훅 (`useNavigate()`)으로 홈페이지로 이동하게 된다.

<br>

### 🧷 File Upload Component

- components/FileUpload.jsx
- `npm install react-dropzone`

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/02eee6d7-8800-4618-aa5b-4be0c3b93362)

- UploadPage 안에 FileUpload Component를 추가하여 이미지 State를 배열형식으로 추가하여 UploadPage 에 전달해준다.
- 그 후, Submit handler 로 Server에 넘겨주고 Server는 Upload 전체 파일 (이미지 포함)을 MongoDB에 전달해준다.

<br>

[https://react-dropzone.js.org/](https://react-dropzone.js.org/)

```jsx
import React from "react";
import Dropzone from "react-dropzone";

<Dropzone
  onDrop={(acceptedFiles) => console.log("acceptedFiles: ", acceptedFiles)}
>
  {({ getRootProps, getInputProps }) => (
    <section>
      <div {...getRootProps()}>
        <input {...getInputProps()} />
        <p>Drag 'n' drop some files here, or click to select files</p>
      </div>
    </section>
  )}
</Dropzone>;
```

![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/37bebc28-e867-4674-b90f-841c77f5d3cc){: width=100% height=100% .normal}
![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/753f8352-c252-454e-9236-5bcefa63531d){: width=100% height=100% .normal}

- React Dropzone을 사용해서 이미지를 업로드하는 방법이다. Drag and drop 혹은 GIF 파일처럼 폴더에서 첨부도 가능하다.
- 밑의 사진의 경우 Array[0]에 File이 들어가 있는 것을 확인할 수 있다.

<br>

```jsx
<div>
  {images.map((image) => {
    <div key={image}>
      <img src={`${import.meta.env.VITE_SERVER_URL}/${image}`} alt={image} />
    </div>;
  })}
</div>
```

- `images` 배열에 포함된 각 이미지를 mapping 하여 이미지를 화면에 표시해준다.
- 각 이미지는 고유한 키(prop)를 가져야한다.
- `import.meta.env.VITE_SERVER_URL` 는 Vite의 환경 변수를 의미한다. 기본적으로 `.env` 파일에서 환경 변수를 로드하여 `import.meta.env` 객체를 통해 엑세스 할 수 있게 해준다.
  - `.env` 에는 서버에서 사용하는 주소를 작성해준다. (`VITE_SERVER_URL=(serverURL)`)
- `${images}` 는 이미지 파일 경로를 보여준다.
- 즉, `import.meta.env.VITE_SERVER_URL/${image}` 는 이미지를 서버에서 가져오기 위한 절대적인 URL을 생성하는 것이다.

<br>

#### 📌 handleDrop

```jsx
const handleDrop = async (files) => {
  // 1. 새로운 FormData 객체를 생성하여 파일 담을 준비
  let formData = new FormData();

  // 2. header 객체를 설정하여 요청의 헤더에 content-type을 multipart/form-data로 설정
  const config = {
    header: { "content-type": "multipart/form-data" }
  };

  // 3. 드롭된 파일 중 첫 번째 파일만을 formData에 추가
  formData.append("file", files[0]);

  try {
    // 4. axiosInstance를 사용하여 서버에 POST 요청을 보낸다.
    //이때 "/products/images" 엔드포인트에 formData를 전송하고, 위에서 설정한 config 객체를 함께 전달.
    const response = await axiosInstance.post(
      "/products/images",
      formData,
      config
    );

    // 5. 서버로부터 응답이 오면, 응답 데이터에서 파일 이름을 가져와서
    // onImageChange 함수를 호출하여 이미지 목록에 새로운 파일 이름을 추가.
    onImageChange([...images, response.data.fileName]);
  } catch (error) {
    console.error(error);
  }
};
```

- (2) `content-type` 특정 헤더 필드에 대한 유효한 값으로 HTTP 요청의 본문 (body)의 내용 유형을 지정한다.
- (2) `multipart/form-data` 는 파일과 함께 폼 데이터를 전송할 때 사용되는 형식이다. (여러 데이터 함께 전송 허용)
  - 앞서 이미지에 파일 업로드 Submit 할 경우 파일 업로드와 같이 복잡한 파일을 포함한 데이터를 전송할 때 사용된다. 😱
- (3) `formData.append("file", files[0]);` 사용자가 한 번에 하나의 파일만 업로드하게 한다.
  - 여러 파일을 동시에 하기 위해선 `files` 배열을 순회하고 각 파일을 FormData에 추가할 수 있음.
- (5) `UploadProductPage` 에서는 `handleImages` 함수 안에 인자로 onImageChange를 넘겨준다.

```jsx
<FileUpload images={product.images} onImageChange={handleImages} />
```

```jsx
const handleImages = (newImages) => {
  setProduct((prevState) => ({
    ...prevState,
    images: newImages
  }));
};
```

<br>

#### 📌 handleDelete

```jsx
const handleDelete = (image) => {
  const currentIdx = images.indexOf(image);
  let newImages = [...images];
  newImages.splice(currentIdx, 1); 
  // currentIndex 부터 1 까지 즉, currentIndex 하나만 지우게 된다.
  onImageChange(newImages);
};
```

- 삭제할 이미지를 매개변수로 받는다.
- 삭제할 이미지의 인덱스를 반환하기 위해 `indexOf`를 사용한다.
- `splice`메서드는 원본 배열을 직접 수정하는 것이 아니라 새로운 배열을 반환하여 불변성을 유지한다.
- `onImageChange` 함수를 통해 상위 컴포넌트로 전달하여 업데이트 한다.

```jsx
{
  images.map((image, index) => (
    <div onClick={() => handleDelete(image)} key={image}>
      <img src={`${import.meta.env.VITE_SERVER_URL}/${image}`} alt={image} />
    </div>
  ));
}
```

- `image.map()` 함수를 사용하여 이미지 배열을 매핑한다.
- Click 이벤트 발생하면 `handleDelete` 함수가 호출되어 해당 이미지를 삭제하고 변경도니 이미지 배열이 상위 컴포넌트로 전달된다.

<br>

> ## Back

### 🧷 product Router

```javascript
// Upload Product
ProductRouter.post("/", auth, async (req, res, next) => {
  try {
    const product = new Product(req.body);
    product.save();
    return res.sendStatus(201);
  } catch (error) {
    next(error);
  }
});
```

![GIF](https://github.com/JEONGSUJONG/readme-main/assets/142254876/e14d1469-196f-46f7-9ac0-9b7ce4fe36f0){: width=100% height=100% .w-50 .normal}

<br>

```javascript
const { Schema } = mongoose;

const productSchema = mongoose.Schema({
  writer: {
    type: Schema.Types.ObjectId,
    ref: "User",
  },
```

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/a059986c-ae7a-4cde-af56-fef65048c8a7){: width=100% height=100% .normal}
![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/b046d159-f7f8-48a1-80af-c0f1c2aae6a7){: width=100% height=100% .normal}

- `writerId` 값과 `userId` 값이 일치함을 볼 수 있다. 👍
- `continents` 값은 Front에서 설정한 값과 일치하구나 🤔

```jsx
const continents = [
  { key: 1, value: "Africa" },
  { key: 2, value: "Europe" },
  { key: 3, value: "Asia" },
  { key: 4, value: "North America" },
  { key: 5, value: "South America" },
  { key: 6, value: "Australia" },
  { key: 7, value: "Antarctica" }
];
```

<br>

### 🧷 Multer

![image](https://github.com/JEONGSUJONG/readme-main/assets/142254876/b6556dc3-361e-44b7-8839-3a4b12a8f2a4)

- `Upload` 폴더 안에 이미지를 업로드 해줄 예정 🥱

```javascript
// Image
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, "uploads/");
  },
  filename: function (req, file, cb) {
    cb(null, `${Date.now()}_${file.originalname}`);
  }
});

const upload = multer({ storage: storage }).single("file");
// "file" 은 FrontEnd의 formData.append("file", files[0]);  이름과 일치해야함
```

- multer 설정하기 : `multer.diskStorage()` 함수를 사용하여 파일이 저장될 디렉토리를 지정해준다. (`destination` 사용)
- upload 미들웨어 설정 : multer 미들웨어에서 생성한 `storage`로 파일을 저장한다.
  - `single("file")` 은 한 번에 하나의 파일만 업로드할 수 있도록 지정한다.

```javascript
ProductRouter.post("/image", auth, async (req, res, next) => {
  upload(req, res, (err) => {
    if (err) {
      return req.status(500).send(err);
    }
    return res.json({ fileName: res.req.file.filename });
  });
});
```

<br>

> FrontEnd의 `handleDelete` 는 `Product` 배열에서만 삭제가 되지 실제 DB에서는 삭제되지 않음 (스케줄링 필요 .. 😭)
{: .prompt-danger }

> 즉, 사용하지 않는 이미지를 확인하고 DB에서 삭제시켜야한다. (일관성 문제 발생, 저장공간 낭비 우려)
{: .prompt-tip }