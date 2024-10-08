---
title: 🔯 React | Query [2]
date: 2024-07-02 00:00:00 +0800
toc: true
pin: false
published: true
categories: [CLIENT, REACT-QUERY]
tags: [react]
image: https://github.com/user-attachments/assets/5c630267-1c97-47e8-93c7-03b0234c3b6e
---

<br>

---

<br>

react query를 사용하면서 익숙해지기 위함이고 대충 싱글페이지로 구현할 예정.
사실 예정은 아니고 이미 만들었음. 

![react-query](https://github.com/user-attachments/assets/2a29247b-8738-4ae8-9782-23c75c66e8cb)

잘 안보이는데 😢 각 컴포넌트 별로 상태관리가 이루어짐을 알 수 있다! 

<br>

> ## FastAPI

react query 게시글이기에... 자세하게 설명은 안하겠음!

```python
# POST /api/v1/post
@post_router.post("/post", response_model=PostModel.Post)
def create_post(post: PostModel.PostCreate, db: Session = Depends(get_db)):
    return PostService.create_post(post, db)

# GET /api/v1/post
@post_router.get("/post", response_model=List[PostModel.Post])
def get_all_posts(db: Session = Depends(get_db)):
    return PostService.get_all_posts(db)

# GET /api/v1/post/{postId}
@post_router.get("/post/{postId}", response_model=PostModel.Post)
def get_post_by_id(postId: int, db: Session = Depends(get_db)):
    return PostService.get_post_by_id(postId, db)

# PUT /api/v1/post/{postId}
@post_router.put("/post/{postId}", response_model=PostModel.Post)
def update_post(postId: int, post: PostModel.PostCreate, db: Session = Depends(get_db)):
    return PostService.update_post(postId, post, db)

# DELETE /api/v1/post/{postId}
@post_router.delete("/post/{postId}")
def delete_post(postId: int, db: Session = Depends(get_db)):
    return PostService.delete_post(postId, db)
```

기본적으로 게시글 생성, 게시글 가져오기, 게시글 수정, 게시글 삭제 (CRUD)

<br>

> ## React

![image](https://github.com/user-attachments/assets/2abe4abc-9347-426d-b21c-9cad40f76cfb){: width="350" height="350" .normal}

<br>

### 📟 `src/api/post-api.ts`

```typescript
const API_URL = "http://127.0.0.1:8000/api/v1/post";

export type Post = {
  id?: string;
  title: string | undefined;
  content: string | undefined;
  created_at?: string;
};

// POST /api/v1/post
export async function createPost(post: Post) {
  const response = await fetch(API_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(post),
  });
  if (response.ok) {
    return await response.json();
  }
  throw new Error("Something went wrong");
}

// GET /api/v1/post
export async function getPosts() {
  const response = await fetch(API_URL);
  if (response.ok) {
    return await response.json();
  }
  throw new Error("Something went wrong");
}

// GET /api/v1/post/{postId}
export async function getPostById(postId: string) {
  const response = await fetch(`${API_URL}/${postId}`);
  if (response.ok) {
    return await response.json();
  }
  throw new Error("Something went wrong");
}

// DELETE /api/v1/post/{postId}
export async function deletePost(postId: string) {
  const response = await fetch(`${API_URL}/${postId}`, {
    method: "DELETE",
  });
  if (response.ok) {
    return await response.json();
  }
  throw new Error("Something went wrong");
}

// PUT /api/v1/post/{postId}
export async function updatePost(postId: string, post: Post) {
  const response = await fetch(`${API_URL}/${postId}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(post),
  });
  if (response.ok) {
    return await response.json();
  }
  throw new Error("Something went wrong");
}

```

### 📟 `src/components/Post-Create.tsx`

```typescript
  const queryClient = useQueryClient();     // useQueryClient를 통해 Query Client 인스턴스를 가져옴
  const mutation = useMutation({            // useMutation을 통해 mutation을 정의함
    mutationFn: createPost,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["posts"] });
    },
  });

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {     // Form이 제출될 때 실행되는 함수 정의
    event.preventDefault();
    const form = event.target as HTMLFormElement;
    const formData = new FormData(form);
    const post: Post = {
      title: formData.get("title") as string,
      content: formData.get("content") as string,
    };
    mutation.mutate(post);      // 정의된 mutation 함수를 호출하여 게시글을 생성함
    form.reset();
  };
```

1. 사용자가 폼에 제목(`title`)과 내용(`content`)을 입력
2. 폼을 제출하면 `handleSubmit` 함수가 호출
3. 입력된 데이터로 게시글 객체를 생성하고, `mutation.mutate(post)`를 호출하여 게시글을 서버에 생성
4. 게시글이 성공적으로 생성되면 `onSuccess` 콜백이 실행되어 ["posts"] 키를 가진 쿼리의 캐시를 무효화. 이를 통해 게시글 목록이 최신 상태로 업데이트


<br>

### 📟 `src/components/Post-List.tsx`

- 모든 게시글을 조회하고 조회된 데이터 중 삭제할 수 있는 기능이 포함됨.

```typescript
  const queryClient = useQueryClient();

  const { data, isLoading, isError } = useQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
  });

  const mutation = useMutation({
    mutationFn: deletePost,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["posts"] });
    },
  });

  const handleDelete = (postId: string) => {
    mutation.mutate(postId);
  };
```

- 모든 게시물 불러오기 : queryKey가 `posts` 인 것을 Get하는 것은 상당히 쉬움. api를 호출하고 조회된 `data`를 map함수 돌려서 화면에 뿌려줌.
- 게시글 삭제하기 : handleDelete를 통해 정의된 mutation을 호출함.

<br>

### 📟 `src/components/Post-Detail.tsx`

```typescript
  const [inputValue, setInputValue] = useState("");
  const [postId, setPostId] = useState<string | null>(null);

  const handleSearch = () => {
    setPostId(inputValue);
    setInputValue("");
  };

  const { data, isLoading, isError } = useQuery({
    queryKey: ["posts", postId],
    queryFn: () => (postId ? getPostById(postId) : Promise.resolve(null)),
    enabled: !!postId,      // postId가 없으면 실행하지마!!!
  });
```

- `queryKey: ["posts", postId]` : 여기서 `postId`는 queryKey 배열에서 쿼리를 고유하게 식별하기 위함임. `queryFn`에서 postId를 추출할 수 있음

<br>

### 📟 `src/components/Post-Update.tsx`

```typescript
  const [inputValue, setInputValue] = useState("");
  const [postId, setPostId] = useState<string | null>(null);
  const [formData, setFormData] = useState({ title: "", content: "" });
  const queryClient = useQueryClient();

  const handleSearch = () => {
    setPostId(inputValue);
    setInputValue("");
  };

  const handleInputChange = (
    event: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    const { name, value } = event.target;
    setFormData({ ...formData, [name]: value });
  };

  const mutation = useMutation({
    mutationFn: (updatedPost: Post) => {
      const postId = data?.id;
      return updatePost(postId, updatedPost);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["posts"] });
      setFormData({ title: "", content: "" });      // Form 제출시 상태 초기화
    },
  });

  const { data, isLoading, isError } = useQuery({
    queryKey: ["posts", postId],
    queryFn: () => (postId ? getPostById(postId) : Promise.resolve(null)),
    enabled: !!postId,      // postId가 없으면 실행하지마!!!
  });

  useEffect(() => {
    if (data) {
      setFormData({ title: data.title, content: data.content });
    }
  }, [data]);

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    mutation.mutate(formData);
  };
```

<br>

결론, 복잡한 데이터 fetch 로직을 단순하게 작성할 수 있었음. 각 컴포넌트에서 queryKey를 사용하여 데이터를 쉽게 캐싱하고 여러 번 요청하지 않아서 좋았음. (일관성 유지하기 좋을 듯) 앞으로 주로 사용할 거 같고 아주 마음에 듦 👍
