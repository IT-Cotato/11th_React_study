# React에서 렌더링이 두 번 발생하는 이유 (Strict Mode 관련)

React 앱을 개발할 때 컴포넌트가 **두 번 렌더링**되는 현상을 관찰한 적이 있다면, 이는 대개 **React.StrictMode** 때문입니다. 이 문서에서는 해당 현상이 발생하는 이유와 그 의도에 대해 설명합니다.

---

## ✅ StrictMode란?

`React.StrictMode`는 React에서 **잠재적인 문제를 감지하고 경고**하기 위해 제공하는 개발 도구입니다. 이 모드는 **개발 환경에서만** 활성화되며, **프로덕션 환경**에는 영향을 주지 않습니다.

```jsx
import React from "react";

function App() {
  return (
    <React.StrictMode>
      <MyComponent />
    </React.StrictMode>
  );
}
```

---

## 🔁 렌더링이 두 번 발생하는 이유

StrictMode는 다음과 같은 **사이드 이펙트**를 감지하기 위해 일부 생명주기 메서드와 함수를 **두 번 호출**합니다.

### 주요 원인:

1. **부작용(effect) 검출**
   - `useEffect`, `useLayoutEffect`에서 부적절한 정리(clean-up) 또는 순서 문제를 감지하기 위해 의도적으로 두 번 실행합니다.
2. **의도치 않은 상태 변경 탐지**

   - 컴포넌트 내부에서 상태를 예기치 않게 변경하는 코드를 감지합니다.

3. **컴포넌트의 순수성 검증**
   - 같은 props와 state로 컴포넌트를 다시 호출했을 때 동일한 결과가 나오는지 검증합니다.

---

## 🧩 Mount와 Unmount란?

React 컴포넌트는 생명주기(lifecycle)를 갖습니다. 그중 가장 중요한 두 가지 단계는 다음과 같습니다:

- **Mount (마운트)**  
  컴포넌트가 **DOM에 삽입되는 시점**입니다. 이때 `useEffect`의 첫 번째 콜백 함수가 실행됩니다.

- **Unmount (언마운트)**  
  컴포넌트가 **DOM에서 제거되는 시점**입니다. 이때 `useEffect`의 **cleanup 함수(반환된 함수)** 가 실행됩니다.

이 과정을 통해 React는 컴포넌트의 부작용 관리가 적절히 되고 있는지를 검사할 수 있습니다.

---

## 🛠️ 실제 예시

```jsx
function MyComponent() {
  React.useEffect(() => {
    console.log("✅ Mount!");

    return () => {
      console.log("❌ Unmount!");
    };
  }, []);

  return <div>Hello</div>;
}
```

이 컴포넌트는 StrictMode 안에서 실행될 경우 다음과 같은 로그가 출력됩니다:

```
✅ Mount!
❌ Unmount!
✅ Mount!
```

이것은 실제로 언마운트와 마운트가 두 번 일어나는 것이 아니라, **부작용 검사를 위한 시뮬레이션**입니다. StrictMode는 `useEffect`의 정리(clean-up) 로직이 잘 작동하는지를 확인합니다.

---

## 📍 영향을 받는 함수/메서드

- `constructor`
- `render`
- `setState`
- `useState`
- `useEffect`
- `useMemo`
- `useCallback`

StrictMode는 **다음 함수들을 연속해서 두 번 실행**해 예상치 못한 부작용을 미리 찾아낼 수 있도록 도와줍니다.

---

## 🧪 왜 이런 검사를 하나요?

React 팀은 다음과 같은 문제를 **개발 단계에서 미리 감지**하길 원합니다:

- 메모리 누수
- 잘못된 effect 정리
- 안정성 낮은 코드 패턴

StrictMode를 통해 개발자는 이러한 문제를 **배포 전에 해결**할 수 있습니다.

---

## 🚫 프로덕션에서는 발생하지 않음

걱정하지 마세요! 이중 호출은 오직 **개발 모드에서만** 발생합니다. 프로덕션 빌드에서는 StrictMode의 영향 없이 컴포넌트가 정상적으로 **한 번만** 렌더링됩니다.

---

## 🔚 요약

- `StrictMode`는 잠재적인 버그를 찾기 위한 React의 개발 도구입니다.
- 렌더링, 마운트, 언마운트 등의 사이클을 의도적으로 두 번 실행합니다.
- `useEffect`의 정리(clean-up) 함수가 적절히 동작하는지를 확인하기 위해 mount-unmount-mount 시퀀스를 수행합니다.
- 프로덕션 환경에는 영향을 주지 않습니다.
- 장기적으로 더 안정적이고 예측 가능한 애플리케이션을 위한 설계입니다.

---

📚 참고: [React 공식 문서 – Strict Mode](https://ko.legacy.reactjs.org/docs/strict-mode.html)
