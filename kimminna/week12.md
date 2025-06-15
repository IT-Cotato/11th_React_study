리액트 19 이후부터는 use server, use client라는 두 가지의 중요한 지시어가 도입되었다.

이 둘은 컴포넌트가 서버에서 렌더링될지, 클라이언트에서 렌더링될지를 명시적으로 지정하는 역할을 한다.

리액트의 서버 컴포넌트와 클라이언트 컴포넌트 개념과 밀접하게 연관되어 있으며, 이들을 통해 SSR과 CSR을 효율적으로 혼합하여 사용할 수 있다.

# use server

컴포넌트를 서버에서 렌더링해야 함을 나타낸다. 이 지시어가 선언된 컴포넌트는 서버에서만 실행되고, 클라이언트로 전송된 이후에는 재실행되지 않는다. 이를 통해서 서버 쪽에서 처리해야 하는 로직(디비 쿼리, 비즈니스 로직 등)을 서버 컴포넌트에서 안전하게 구현할 수 있다.

```jsx
// 서버 액션 예시
"use server";

export async function saveUser(formData) {
  const name = formData.get("name");
  await db.insert({ name });
}
```

# use client

컴포넌트를 클라이언트에서 렌더링해야 함을 나타낸다. 이 지시어가 선언된 컴포넌트는 클라이언트 사이드에서만 실행되며, 브라우저 환경에서 필요한 로직(ui 상호작용, 상태 관리 등)을 처리할 수 있다.

```jsx
// 클라이언트 컴포넌트
"use client";

export default function ClientComponent() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Click {count}</button>;
}
```

# 결합하여 사용하기

리액트 19에서는 서버와 클라이언트 컴포넌트를 결합하여 사용해서 복잡한 애플리케이션의 퍼포먼스를 최적화할 수 있게 되었다. 예를 들어, 데이터 페칭 로직을 서버에서 처리하고, 그 데이터를 클라이언트 컴포넌트에 전달하여 ui 상호작용을 처리하는 구조를 쉽게 구현할 수 있다.

```jsx
// actions/saveUser.ts
"use server";

export async function saveUser(formData) {
  const name = formData.get("name");
  await db.insert({ name });
}

// components/UserForm.tsx
("use client");

import { saveUser } from "../actions/saveUser";

export default function UserForm() {
  return (
    <form action={saveUser}>
      <input name="name" />
      <button type="submit">Save</button>
    </form>
  );
}
```

| 구분      | `"use client"`             | `"use server"`                  |
| --------- | -------------------------- | ------------------------------- |
| 정의      | 클라이언트 컴포넌트로 명시 | 서버 함수(Server Action)로 명시 |
| 위치      | 컴포넌트 상단              | 함수 상단 or 선언부             |
| 주요 역할 | 상태관리, 이벤트 처리      | DB, API, 인증 처리              |
| 사용 대상 | UI 컴포넌트                | 서버 액션 함수                  |
| 안전성    | 번들에 포함됨 (노출됨)     | 서버 전용 (보안 안전)           |
