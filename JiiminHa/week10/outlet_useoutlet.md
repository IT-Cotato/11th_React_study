# React Router: Outlet & useOutlet 완전 정리

## 1. Outlet이란?

`<Outlet />`은 중첩된 라우트의 컴포넌트를 렌더링하는 위치를 지정하는 컴포넌트

예를 들어, `/mypage`에 공통 레이아웃이 있고 `/mypage/info`, `/mypage/settings` 같은 하위 페이지가 있을 때,  
공통 레이아웃 컴포넌트에서 `<Outlet />`을 사용해 자식 라우트를 표시 가능

### ✅ 예시: Layout 컴포넌트

```jsx
// Layout.jsx
import { Outlet } from "react-router-dom";

export default function Layout() {
  return (
    <div>
      <Navbar />
      <Outlet /> {/* 자식 라우트가 여기에 렌더링됩니다 */}
      <Footer />
    </div>
  );
}
```

---

## 2. useOutlet이란?

`useOutlet()`은 현재 `<Outlet />`에 렌더링될 React 엘리먼트를 반환하는 Hook.  
즉, Outlet의 내용을 직접 변수로 가져와서 **조건부 렌더링**이나 **특정 Wrapper로 감싸고 싶을 때** 유용하게 사용.

### ✅ 예시: 로그인된 사용자만 Outlet을 렌더링

```jsx
// ProtectedLayout.jsx
import { useOutlet } from "react-router-dom";
import { useAuth } from "./hooks/useAuth";

export default function ProtectedLayout() {
  const outlet = useOutlet();
  const isLoggedIn = useAuth();

  if (!isLoggedIn) return <Navigate to="/login" />;
  return <div className="protected">{outlet}</div>;
}
```

---

## 3. 중첩 라우트 구성 예시

중첩 라우트는 `Route` 안에 `Route`를 넣어 구성하며, 부모 라우트의 컴포넌트가 `<Outlet />`을 포함해야 함.

```jsx
// App.jsx
import { Routes, Route } from "react-router-dom";
import Layout from "./Layout";
import Info from "./pages/Info";
import Settings from "./pages/Settings";

function App() {
  return (
    <Routes>
      <Route path="/mypage" element={<Layout />}>
        <Route path="info" element={<Info />} />
        <Route path="settings" element={<Settings />} />
      </Route>
    </Routes>
  );
}
```

---

## 4. Outlet vs useOutlet 요약

| 항목      | Outlet                  | useOutlet                             |
| --------- | ----------------------- | ------------------------------------- |
| 형태      | 컴포넌트 (`<Outlet />`) | Hook (`useOutlet()`)                  |
| 용도      | 자식 라우트 표시 위치   | 자식 라우트를 직접 변수로 가져와 제어 |
| 사용 시기 | 일반적인 중첩 라우트    | 인증, 조건부 렌더링 등에서 유용       |

---

## 5. 정리

- `Outlet`은 자식 라우트를 표시하는 위치를 지정.
- `useOutlet`은 Outlet에 들어올 컴포넌트를 직접 변수처럼 사용할 수 있게 함.
- 중첩 라우트 구조를 깔끔하게 만들고, 인증 처리나 조건부 UI 구성에 매우 유용

---

## 6. 참고 링크

- React Router 공식 문서: https://reactrouter.com/en/main/start/overview
- Nested Routes 설명: https://reactrouter.com/en/main/start/concepts#nested-routes
