## `useRef`란?

* `useRef()`는 `{ current: 값 }` 형태의 객체를 반환!
* 컴포넌트가 리렌더링되어도 값이 유지된다는 장점
* 상태 변경 없이 값을 저장하거나 DOM에 직접 접근할 때 사용

---

## 활용 예시

### 1. 값 저장 (리렌더링 없이)

```js
const idRef = useRef(3);  // Todo의 고유 ID 관리
```

* `useState`와 달리 값이 변경되어도 리렌더링되지 않는다
* 상태 변화 없이 값을 누적하거나 추적할 때 유용
  

---

### 2. DOM 접근

```js
const inputRef = useRef(null);

useEffect(() => {
  inputRef.current.focus();
}, []);
```

* `ref={inputRef}`로 HTML 요소에 직접 접근할 수 있음
* 예: input 포커스 제어, 스크롤 위치 이동, 애니메이션 효과 적용 등

---

## 구체적인 예시

### 1. 입력창 포커스 제어

```jsx
function FocusInput() {
  const inputRef = useRef(null);
  const focus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="클릭하면 여기 활성화" />
      <button onClick={focus}>포커스 버튼</button>
    </div>
  );
}
```
useState는 화면을 다시 그려주는데, 화면을 바꾸지 않고 값만 바꾸고 싶을 때 useRef 사용

---

### 2. `setInterval` 관리

```js
function Timer() {
  const timerId = useRef(null);

  const startTimer = () => {
    timerId.current = setInterval(() => {
      console.log("타이머 실행 중!");
    }, 1000);
  };

  const stopTimer = () => {
    clearInterval(timerId.current);
  };
}
```

* 타이머 ID를 상태 대신 ref에 저장하여 불필요한 리렌더링 방지

---

### 3. 애니메이션 DOM 제어

```js
function AnimatedBox() {
  const boxRef = useRef(null);

  const moveBox = () => {
    boxRef.current.style.transform = "translateX(100px)";
  };

  return (
    <div>
      <div ref={boxRef} style={{ width: "50px", height: "50px", background: "red" }} />
      <button onClick={moveBox}>이동</button>
    </div>
  );
}
```
* DOM 접근
애니메이션이나 특정 라이브러리를 사용해서 DOM에 접근해야 할때 useRef 를 사용하면 HTML 요소를 가리키는 참조를 생성할 수 있음
---

### 4. 폼 데이터 저장

```js
function Form() {
  const formData = useRef({ name: "", email: "" });

  return (
    <div>
      <input onChange={(e) => (formData.current.name = e.target.value)} />
      <input onChange={(e) => (formData.current.email = e.target.value)} />
      <button onClick={() => console.log(formData.current)}>제출</button>
    </div>
  );
}
```
* 폼 데이터의 효율적인 관리
  
useState를 사용하면 입력값이 변경될 때마다 리렌더링이 발생
useRef를 사용하면 폼 데이터를 저장하고 수정하더라도 화면에 영향을 주지 않음!
폼 입력은 사용자의 타이핑에 따라 자주 변경되기에 불필요한 리렌더링을 방지하려면 useRef 를 사용!

