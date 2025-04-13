# React에서의 Prop Drilling

## Prop Drilling이란?

Prop Drilling은 React 애플리케이션에서 상위 컴포넌트가 가진 데이터를 하위 컴포넌트까지 전달해야 할 때, 그 중간에 위치한 컴포넌트들이 해당 데이터를 직접 사용하지 않더라도 props로 전달받아 다시 넘겨주는 구조를 말합니다.

즉, 실제로 데이터를 필요로 하지 않는 중간 컴포넌트들이 단순히 데이터 전달의 매개 역할만 하게 되며, 이로 인해 코드의 복잡성과 유지보수 비용이 증가하게 됩니다.

## 예시

```jsx
function GrandParent() {
  const user = "지민";
  return <Parent user={user} />;
}

function Parent({ user }) {
  return <Child user={user} />;
}

function Child({ user }) {
  return <p>안녕하세요, {user}님!</p>;
}
```

위 예시에서 `Child` 컴포넌트는 `user`라는 데이터를 필요로 합니다. 그러나 `GrandParent`에서 `Child`까지 데이터를 전달하기 위해 `Parent` 컴포넌트가 props를 중간에 전달만 하고 있습니다. 이처럼 중간 컴포넌트가 필요하지 않은 데이터를 계속 전달하게 되는 것이 prop drilling의 대표적인 예입니다.

## 문제점

- 중간 컴포넌트가 불필요한 props를 받아야 하므로 컴포넌트 재사용성이 낮아집니다.
- 데이터가 깊게 전달되다 보면 변경 사항을 추적하고 수정하는 데 어려움이 생깁니다.
- 코드 가독성과 유지보수가 어려워집니다.

## 해결 방법

### 1. React Context API 사용

React의 Context API를 사용하면 중간 컴포넌트를 거치지 않고 원하는 하위 컴포넌트에서 직접 데이터를 사용할 수 있습니다.

```jsx
import React, { createContext, useContext } from "react";

const UserContext = createContext();

function GrandParent() {
  return (
    <UserContext.Provider value="지민">
      <Parent />
    </UserContext.Provider>
  );
}

function Parent() {
  return <Child />;
}

function Child() {
  const user = useContext(UserContext);
  return <p>안녕하세요, {user}님!</p>;
}
```

Context API는 전역적으로 접근 가능한 데이터를 정의하고, 필요한 컴포넌트에서만 직접 접근하도록 해 prop drilling 문제를 해결합니다.

### 2. 상태 관리 라이브러리 사용

Redux, Recoil, Zustand, Jotai 등의 상태 관리 라이브러리를 활용해도 prop drilling을 피할 수 있습니다. 이들은 전역 상태를 관리해 필요한 컴포넌트에서만 접근 가능하게 해줍니다.

## 결론

Prop Drilling은 작은 프로젝트에서는 문제가 되지 않을 수 있지만, 규모가 커지면 컴포넌트 간 데이터 흐름이 복잡해지고 유지보수가 어려워집니다. React에서는 Context API 또는 상태 관리 도구를 활용해 이 문제를 구조적으로 해결할 수 있습니다.
