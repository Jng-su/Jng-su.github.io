---
title: 🔯 React - Query [1]
date: 2024-07-01 00:00:00 +0800
toc: true
pin: false
published: true
categories: [CLIENT, REACT-QUERY]
tags: [react, query, fastapi]
image: https://github.com/user-attachments/assets/5c630267-1c97-47e8-93c7-03b0234c3b6e
---

<br>

---

<br>

| Backend | FastAPI, SQLite |
| Frontend | React + Vite    |

<br>

> ## Initial Setting

<br>

### Client

- client 폴더 접속

    ```shell
    cd client
    ```

- vite 환경설치

    ```shell
    npm init vite
    ```

    - project name : `.`
    - select a framework : `React`
    - select a variant : `typescript`


- 의존성 설치

    ```shell
    npm install
    ```


- 설치 확인

    ```shell
    npm run dev
    ```

<br>

### Server


- server 폴더 접속

    ```shell
    cd server
    ```

- 가상환경 생성

    ```shell
    python -m venv ./venv
    ```


- 가상환경 접속

    ```shell
    source ./venv/Scripts/activate
    ```


- FastAPI, uvicorn

    ```shell
    pip insatll fastapi "uvicorn[standard]"
    ```


- SqlModel 설치

    ```shell
    pip install sqlmodel
    ```


<br>

> ## React Query


React에서 서버에 데이터를 요청해서 가져오기, 데이터를 서버에 업데이트, 캐싱 작업 등을 하기 쉽게 도와주는 라이브러리

1. 서버의 상태를 생성하는 `isLoading`, `isError` 등 굳이 생성하지 않아도 됨.
2. 직접 해보니 기존 Redux 상태관리 라이브러리보다 코드가 간결해짐.
3. 캐싱 기능이 있어 불필요한 서버 요청을 줄일 수 있음.

<br>

### Install Tanstack-ReactQuery

```shell
npm install @tanstack/react-query
```

<br>

#### useQuery

서버로부터 데이터를 요청하여 받아오는 Get

```typescript
const { data } = useQuery(QueryKey, QueryFn, Options..)
```

  - `QueryKey` : 유니크한 쿼리 키값을 사용하여 다른 곳에서도 동일한 쿼리를 불러올 때 사용함. 즉, 리액트 쿼리가 쿼리 데이터를 판별할 때 사용됨
  - `QueryFn` : useQuery를 통해서 쿼리를 정의할 때 전달하는 함수
  - `Options` : 쿼리에서 사용될 옵션을 정의 (enable, staleTime, cashTime, onSuccess, onError, onSettled, retry ...)

<br>

#### useQueries

위의 `useQuery`와 유사하지만 "복수형"임.

```typescript
const res = useQueries({
    queries : [
        { queryKey : ["post"], queryFn : getPost }
        { queryKey : ["board"], queryFn : getBoard }
    ]
})
```

<br>

#### useQueryClient, useMutation

  - `useQueryClient` : Query Client 인스턴스를 가져오는 데 사용되는 훅. 이 훅을 통해 쿼리 캐시를 제어할 수 있음.
  - `useMutation` : 서버에서 데이터를 변경(mutation)할 때 사용됨. (데이터 생성, 수정, 삭제)

```typescript
const queryClient = useQueryClient();
  const mutation = useMutation({
    mutationFn: createPost,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["posts"] });
    },
  });
```

  - `mutationFn` : mutation이 실행될 때 호출되는 함수
  - `invalidateQueries` : queryKey가 `posts`인 쿼리의 캐시를 무효화하여 데이터들을 최신상태로 유지시켜줌

