# React Router v7: 라우팅 라이브러리에서 앱 흐름 설계 도구로

## ✅ 주제 개요

React Router는 오랫동안 SPA의 라우팅을 담당하는 대표적인 라이브러리였지만, v7에서는 단순한 "URL → 컴포넌트 매핑"을 넘어서 **앱의 흐름을 설계하는 프레임워크 수준의 도구**로 진화했다.

---

## 🚀 주요 변화 핵심 요약

### 1. 데이터 중심 라우팅 (`loader`, `action`)

* 라우트 단위로 데이터를 불러오고, 폼 요청을 처리하는 함수 선언 가능
* 컴포넌트가 mount되기 전에 데이터를 미리 준비함 → SSR & prefetch 구조 가능

```tsx
const router = createBrowserRouter([
  {
    path: "/user/:id",
    element: <UserPage />,
    loader: async ({ params }) => {
      return fetch(`/api/users/${params.id}`).then(res => res.json());
    },
    action: async ({ request }) => {
      const formData = await request.formData();
      return fetch("/api/users", {
        method: "POST",
        body: formData,
      });
    },
  },
]);
```

### 2. 라우트 수준 오류 처리 (`errorElement`)

* 라우트마다 개별적인 에러 처리가 가능해짐

```tsx
<Route
  path="/dashboard"
  element={<Dashboard />}
  errorElement={<DashboardError />}
/>
```

### 3. navigate 함수의 비동기화 (v7 주요 변화)

* 기존 v6까지는 `navigate()`가 **동기 함수** → 즉시 페이지 이동
* v7부터는 **비동기 함수(Promise)** → navigate 호출 직후에도 컴포넌트가 살아 있음

→ 그 결과, navigate 후에도 useEffect 등 React hook이 실행될 수 있음

---

## ❗️실제 이슈 사례: 감정 일기장 프로젝트에서의 navigate 버그

### 문제 상황

1. 삭제 버튼 클릭 시 onDelete → navigate 순서로 호출
2. navigate가 비동기이기 때문에 컴포넌트가 곧바로 언마운트되지 않음
3. 그 사이 data state가 바뀌며 useEffect 재실행 → `존재하지 않는 일기입니다` alert 발생

### 기존 코드

```tsx
useEffect(() => {
  const currentDiaryItem = data.find(item => String(item.id) === String(id));
  if (!currentDiaryItem) {
    alert("존재하지 않는 일기입니다");
    nav("/", { replace: true });
  }
}, [id, data]);
```

### 수정 방법

```tsx
// 의존성 배열을 [id]로 변경해 data 변화에 반응하지 않도록 조정
useEffect(() => {
  const currentDiaryItem = data.find(item => String(item.id) === String(id));
  if (!currentDiaryItem) {
    alert("존재하지 않는 일기입니다");
    nav("/", { replace: true });
  }
}, [id]);
```

---

## 💡 왜 중요한가?

### navigate 비동기화는 단순한 구현 변경이 아니다

→ **라우팅의 타이밍 제어와 컴포넌트 생명주기의 충돌**이라는 더 본질적인 문제와 연결됨

### 이 변화는 무엇을 의미하는가?

* 라우터가 단순히 "경로 바꾸기"가 아닌,
* **앱 상태 변화 흐름을 제어하는 핵심 엔진**이 되었다는 것

---

## 📚 참고

* 관련 강의: 한입 리액트 v7 패치노트
* 실습 예시: 감정 일기장 프로젝트 Edit 페이지 navigate 이슈
