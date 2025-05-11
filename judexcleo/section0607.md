## 리액트 컴포넌트의 라이프사이클

컴포넌트의 생성(Mount), 갱신(Update), 제거(UnMount) 과정을 제어하는 메서드들의 집합

### 1. Mount (탄생)

* 컴포넌트가 처음 화면에 렌더링됨
* 예: A 컴포넌트가 Mount됨 → A가 처음 화면에 나타남

### 2. Update (변화)

* 컴포넌트가 리렌더링되는 시점
* 예: A 컴포넌트가 Update됨 → 상태나 props 변화로 다시 렌더링됨

### 3. UnMount (제거)

* 컴포넌트가 화면에서 사라지는 시점
* 예: A 컴포넌트가 UnMount됨 → 더 이상 렌더링되지 않음

---

## useEffect란

함수형 컴포넌트에서 사이드 이펙트를 처리하기 위한 Hook
사이드 이펙트란 UI 렌더링 외의 작업 (API 호출, 이벤트 리스너 등록, 콘솔 로그, WebSocket 연결 등)을 의미함

### 기본 문법

```js
useEffect(effectFunction, dependencyArray)
```

* effectFunction: 렌더링 이후 실행할 함수
* dependencyArray:

  * `[]`: 마운트 시 한 번만 실행
  * `[state]`: 해당 값이 바뀔 때만 실행
  * 생략: 렌더링마다 실행됨

---

## useEffect 예시

```jsx
useEffect(() => {
  console.log(`count: ${count}`)
}, [count])
```

* `count` 값이 변경될 때마다 로그 출력됨

---

## setState의 비동기성

```jsx
const onClickButton = (value) => {
  setCount(count + value)
  console.log(count)  // 이전 값 출력됨
}
```

* `setCount`는 비동기이므로 바로 다음 줄에서 값이 반영되지 않음
* 정확한 값 확인은 `useEffect`에서 처리해야 함

---

## useEffect를 통한 라이프사이클 제어

### Mount

```js
useEffect(() => {
  console.log("mount")
}, [])
```

### Update

```js
useEffect(() => {
  console.log("update")
})
```

### UnMount

```js
useEffect(() => {
  return () => {
    console.log("unmount")
  }
}, [])
```

* `return` 내부 함수는 클린업 함수로서 언마운트 시 자동 호출됨
