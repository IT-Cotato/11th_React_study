## React 이벤트 기초 개념

1. **카멜케이스 사용**
    - HTML: `onclick` → React: `onClick`
2. **JS 함수 전달**
    - `onClick={handleClick}`처럼 함수 **참조** 전달
        
        (`onClick={handleClick()}` ❌ 즉시 실행됨)
        
3. **SyntheticEvent 사용**
    - React는 자체 이벤트 래퍼인 `SyntheticEvent`를 사용 (브라우저 간 호환성 + 성능 최적화)

```jsx
function handleClick(e) {
  console.log(e); // SyntheticEvent
  console.log(e.nativeEvent); // 브라우저의 실제 이벤트
}
```

---

## 심화 개념

### 1. **SyntheticEvent (합성 이벤트)란?**

- React에서 브라우저 이벤트를 감싸는 래퍼
- `e.preventDefault()`, `e.stopPropagation()` 사용 가능
- 동기식으로 작동하지만, 이벤트 핸들러 끝나면 이벤트 객체는 **자동으로 초기화됨**
→ 그래서 **비동기 처리할 땐 이벤트를 저장**해야 함

```jsx
function handleClick(e) {
  e.persist(); // 초기화 방지
  setTimeout(() => {
    console.log(e.target); // 안전하게 접근 가능
  }, 1000);
}
```

---

### 2. **이벤트 위임**

React는 모든 이벤트를 실제로 **문서(document) 상단에서 듣고,** **버블링**을 활용해 이벤트를 처리

→ 매번 DOM에 직접 이벤트 리스너를 붙이지 않아서 성능이 좋음

---

### 3. **고차 이벤트 핸들러 (커링 패턴)**

컴포넌트마다 다른 파라미터 넘기고 싶을 때 사용

```jsx
const handleClick = (name) => (e) => {
  console.log(`${name} clicked`);
};

<button onClick={handleClick('Jade')}>Click Me</button>
```

---

### 4. **form 요소 이벤트 종류**

| 이벤트 종류 | 설명 |
| --- | --- |
| `onChange` | input 값 변경 시 |
| `onSubmit` | form 제출 시 |
| `onFocus` / `onBlur` | 포커스 진입/이탈 시 |
| `onKeyDown` / `onKeyUp` | 키 입력 시 반응 |

---

### 5. **React에서 기본 동작 막기 / 전파 막기**

```jsx
function handleSubmit(e) {
  e.preventDefault(); // 폼 제출 막기
}

function handleClick(e) {
  e.stopPropagation(); // 상위 요소로 이벤트 전달 막기
}
```

---

### 6. 응용: 이벤트 한 번만 실행하기

```jsx
function handleOnceClick() {
  console.log("한 번만 실행됨");
  setClicked(true); // 예: 상태로 막기
}
```
