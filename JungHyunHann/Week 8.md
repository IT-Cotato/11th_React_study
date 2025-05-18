### 1. **라우팅이란?**

- `Route`(경로) + `ing`(진행): 경로를 지정하는 과정
- 데이터를 목적지로 보내기 위한 **최적의 경로 선택**

---

### 2. **페이지 라우팅이란?**

- 사용자의 URL 요청에 따라 해당 경로의 페이지를 반환하는 과정
- 예: `/home` → Home 페이지, `/about` → About 페이지

---

### 3. **리액트 라우팅 방식**

- **서버 사이드 렌더링 (SSR)**
    1. 웹 브라우저에서 URL로 서비스 요청 
    2. 웹 서버는 요청 URL에서 경로를 확인하고 해당 html을 생성해 반환
    3. 웹 브라우저는 웹 서버에서 반환된 html을 보여줌
    
    → 웹 브라우저에 표시할 페이지를 웹 서비에서 만들어서 전달
    
- **클라이언트 사이드 렌더링 (CSR) (React에서 주로 사용)**
    1. 웹 브라우저에서 URL로 서비스 요청 
    2. 웹 서버는 요청 URL의 경로를 따지지 않고 페이지의 틀 역할을 하는 index. html과 리액트 앱을 함께 반환
    3. 웹 브라우저는 서버에서 제공된 index.html 페이지를 보여주고 리액트 앱 실행
    4. 리액트 앱은 현재 경로에 맞는 페이지를 보여줌
    5. 사용자가 페이지를 이동하면 웹 브라우저는 서버에서 받은 리액트 앱을 실행해 자체적으로 페이지 교환
    
    → 페이지를 브라우저가 직접 만듦
    

---

### 4. 리액트 라우터란?

- 리액트 앱에서 경로별로 컴포넌트를 렌더링하는 라이브러리
- `react-router-dom` 패키지 사용
- [**https://reactrouter.com**](https://reactrouter.com/)
    
    **설치 방법**
    
    ```bash
    npm install react-router-dom
    ```
    
    **프로젝트에 라우터 적용 (`index.js`)**
    
    ```jsx
    import { BrowserRouter } from "react-router-dom";
    
    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(
      <BrowserRouter>
        <App />
      </BrowserRouter>
    );
    ```
    
    - `BrowserRouter`는 브라우저의 주소 변경을 감지하고, 해당 경로에 맞는 컴포넌트를 보여줌

---

### 5. 리액트 라우터의 주요 컴포넌트

- `Routes`: 라우트들을 감싸는 컴포넌트
- `Route`: 개별 경로와 컴포넌트 연결
    - 각각의 `Route`는 `path`와 `element` 속성을 가집니다.

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
```

- `Link`: a 태그 대신 사용하여 새로고침 없이 페이지 이동

```jsx
<Link to="/about">About</Link>
```

- `useNavigate`: 자바스크립트 코드로 경로 이동 처리 가능

```jsx
const navigate = useNavigate();
navigate("/home");
```

---

### 6. URL 파라미터와 쿼리스트링

**동적 라우팅 (URL 파라미터)**

- `:id`처럼 경로에 변수처럼 사용할 수 있음

```jsx
<Route path="/post/:id" element={<PostDetail />} />
```

- 컴포넌트 내에서는 `useParams` 훅으로 파라미터를 추출

```jsx
const { id } = useParams();
```

**쿼리스트링 (Search Params)**

- `?key=value` 형식의 데이터를 URL에 포함시키는 방식
- `useSearchParams` 훅을 이용해 읽기/쓰기 가능

```jsx
const [searchParams] = useSearchParams();
const sort = searchParams.get("sort"); // 예: /list?sort=asc
```

---

### 7. 중첩 라우팅

- 컴포넌트 내부에 또 다른 하위 라우트를 구성할 수 있음

```jsx
<Route path="/mypage" element={<MyPage />}>
  <Route path="profile" element={<Profile />} />
  <Route path="setting" element={<Setting />} />
</Route>
```

- `Outlet` 컴포넌트를 사용해 중첩된 컴포넌트를 렌더링

```jsx
// MyPage.js
return (
  <div>
    <h2>마이페이지</h2>
    <Outlet />
  </div>
);
```

---

### 8. 인증 기반 라우팅 보호

- 로그인 여부에 따라 페이지 접근을 제한할 수 있음

```jsx
function PrivateRoute({ children }) {
  const isLoggedIn = localStorage.getItem("token") !== null;
  return isLoggedIn ? children : <Navigate to="/login" />;
}
```

---

### 9. CSR, SSR, SSG 비교

| 구분 | 설명 | 대표 프레임워크 |
| --- | --- | --- |
| CSR (Client Side Rendering) | 클라이언트에서 렌더링, 초기 로딩 느리지만 전환 빠름 | React |
| SSR (Server Side Rendering) | 서버에서 HTML 생성 후 전송, SEO 유리 | Next.js |
| SSG (Static Site Generation) | 정적 HTML로 미리 생성, 빠르지만 동적 데이터 불리 | Gatsby, Next.js (Static Mode) |
- 리액트는 기본적으로 CSR을 사용하며, SEO(웹사이트가 구글, 네이버 같은 검색 엔진에서 더 잘 검색되도록 만드는 기술이나 전략)가 중요한 경우에는 SSR 기반의 Next.js 사용이 권장됨
