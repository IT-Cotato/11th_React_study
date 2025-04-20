## Controlled vs Uncontrolled Components

React에서 `<input>`, `<textarea>`, `<select>` 같은 폼 요소를 다룰 때, 입력값을 **어떻게 관리하느냐에 따라** 컴포넌트는 다음 두 가지 방식으로 나뉜다.

- Controlled Component (제어 컴포넌트)
- Uncontrolled Component (비제어 컴포넌트)

---

### Controlled Component (제어 컴포넌트)

**입력 요소의 값을 React의 state로 직접 제어하는 방식**

### 특징

- `useState`를 통해 입력값을 상태로 관리
- 입력 값은 `value` 속성으로 설정
- `onChange` 이벤트로 상태 업데이트
- 상태(state)가 진짜 데이터 소스 (source of truth)

### 코드 예시

```jsx
import { useState } from 'react';

function ControlledInput() {
  const [name, setName] = useState(''); // 상태 선언

  return (
    <input
      value={name} // 상태와 입력값을 연결 (Controlled)
      onChange={(e) => setName(e.target.value)} // 입력값이 바뀔 때 상태 업데이트
    />
  );
}

```

### 장점

- 실시간으로 입력값을 확인하거나 유효성 검사 가능
- 입력값에 따라 동적으로 UI를 제어 가능
- 상태 기반 로직을 통일성 있게 관리할 수 있음

### 단점

- 입력값이 많아질수록 상태 선언과 관리가 번거로움
- 입력마다 리렌더링 발생 → 과도한 렌더링으로 성능 저하 우려

### 사용 예시

- 로그인/회원가입 폼
- 실시간 검색 필터
- 체크박스/라디오 버튼 선택 후 UI 변화

---

### Uncontrolled Component (비제어 컴포넌트)

**React가 아닌 브라우저가 입력값을 관리하고, 필요할 때만 DOM에서 값을 읽어오는 방식**

### 특징

- 입력값을 `useRef`로 직접 접근
- `defaultValue`로 초기값 지정
- React 상태를 거치지 않기 때문에 입력만으로는 리렌더링 없음

### 코드 예시

```jsx
import { useRef } from 'react';

function UncontrolledInput() {
  const inputRef = useRef(); // DOM을 직접 참조할 ref 생성

  const handleClick = () => {
    alert(inputRef.current.value); // 현재 입력된 값 가져오기
  };

  return (
    <>
      <input ref={inputRef} defaultValue="초기값" /> {/* 비제어 방식 */}
      <button onClick={handleClick}>확인</button>
    </>
  );
}

```

### 장점

- 코드 간결, 상태 선언 없이도 동작 가능
- 리렌더링이 일어나지 않아 성능 면에서 유리
- 특정 시점(버튼 클릭 등)에만 값이 필요할 때 적합

### 단점

- 실시간 유효성 검사, 조건부 UI 처리 어려움
- 입력 상태를 추적하지 못함

### 사용 예시

- 파일 업로드 (`<input type="file">`)
- 단순 메모 입력창
- 제출 시점에만 값이 필요한 폼

---

### 종합 비교 표

| 항목 | Controlled Component | Uncontrolled Component |
| --- | --- | --- |
| 제어 주체 | React (state) | 브라우저 (DOM) |
| 입력값 접근 | useState | useRef.current.value |
| 초기값 설정 | value 속성 | defaultValue 속성 |
| 리렌더링 | 입력마다 리렌더링 | 리렌더링 없음 |
| 상태 동기화 | 항상 React 상태와 연결됨 | 필요 시 DOM에서 추출 |
| 유효성 검사 | 실시간 검사 가능 | 직접 검사 구현 필요 |
| 코드 양 | 상대적으로 많음 | 상대적으로 간단 |
| 주요 사용 예 | 로그인, 필터 폼 | 파일 업로드, 제출용 입력 |

---

### 정리

- **Controlled 방식**이 React의 기본 폼 처리 방식이며, 대부분의 경우에 권장됨
- 다만, **Uncontrolled 방식**은 입력값 추적이 필요 없고, 성능이 중요하거나 간단한 입력만 필요할 경우 유리함
- 목적과 상황에 따라 두 방식을 적절히 선택하는 것이 중요함
