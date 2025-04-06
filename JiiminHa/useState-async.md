# React의 useState는 왜 비동기적으로 동작하는가?

React의 `useState` 훅은 컴포넌트에 상태 변수를 추가할 수 있도록 해줍니다. 그러나 `useState`를 사용할 때, 상태 업데이트가 즉시 반영되지 않고 비동기적으로 처리된다는 점을 이해하는 것이 중요합니다. 이 문서에서는 그 이유와 주의사항에 대해 알아보겠습니다.

---

## ✅ useState의 동작 방식

`useState`는 상태 변수와 해당 상태를 업데이트할 수 있는 함수를 반환합니다:

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    console.log(count); // 예상과 달리 업데이트된 값이 아닌 이전 값이 출력될 수 있음
  };

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={handleClick}>증가</button>
    </div>
  );
}
```

위의 예시에서 `handleClick` 함수 내에서 `setCount`를 호출한 후 바로 `console.log(count)`를 실행하면, 업데이트된 값이 아닌 이전 값이 출력될 수 있습니다. 이는 `setCount`가 상태 업데이트를 요청하지만, 실제 업데이트는 다음 렌더링에서 반영되기 때문입니다.

---

## 🔁 상태 업데이트의 비동기 처리 이유

React는 성능 최적화를 위해 여러 개의 상태 업데이트를 하나의 렌더링으로 묶어 처리하는 **배칭(batching)** 기법을 사용합니다. 이러한 배칭은 불필요한 렌더링을 방지하고 성능을 향상시키는 데 도움이 됩니다. 따라서 `setState` 호출은 즉시 상태를 업데이트하지 않고, 이벤트 핸들러가 종료된 후 한꺼번에 처리됩니다. ([legacy.reactjs.org](https://legacy.reactjs.org/docs/state-and-lifecycle.html?utm_source=chatgpt.com))

---

## 🛠️ 이전 상태를 기반으로 한 업데이트

상태 업데이트가 이전 상태에 의존하는 경우, 함수형 업데이트를 사용하는 것이 안전합니다. 이를 통해 React는 가장 최신의 상태 값을 보장합니다:

```jsx
const handleClick = () => {
  setCount((prevCount) => prevCount + 1);
};
```

위와 같이 함수형 업데이트를 사용하면, `prevCount`는 항상 최신 상태 값을 참조하므로 상태 업데이트의 정확성을 유지할 수 있습니다. ([legacy.reactjs.org](https://legacy.reactjs.org/docs/hooks-reference.html?utm_source=chatgpt.com))

---

## ⚠️ 주의사항

- **즉시 상태 값을 읽으려는 시도**: `setState` 호출 직후에 상태 값을 읽으면 예상과 다른 결과를 얻을 수 있습니다. 상태 업데이트는 비동기적으로 처리되므로, 업데이트된 값은 다음 렌더링 이후에 반영됩니다.

- **이전 상태에 의존하는 업데이트**: 이전 상태 값을 기반으로 새로운 상태를 계산해야 할 때는 함수형 업데이트를 사용하는 것이 권장됩니다.

---

## 🔚 요약

- `useState`의 상태 업데이트는 비동기적으로 처리되어 성능 최적화를 돕습니다.
- 상태 업데이트 직후에 상태 값을 읽으면 이전 값이 반환될 수 있으므로 주의해야 합니다.
- 이전 상태에 의존하는 업데이트는 함수형 업데이트를 사용하여 정확성을 유지할 수 있습니다.

---

📚 참고: [React 공식 문서 – useState](https://beta.reactjs.org/reference/react/useState)
