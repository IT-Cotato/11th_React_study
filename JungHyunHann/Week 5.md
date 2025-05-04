# useEffect

## useEffect는 언제 실행됨?

> “렌더링 후 실행된다.”
> 
> 
> “의존성 배열에 따라 실행 타이밍이 달라진다.”
> 
- 여기까지는 맞지만, 실제로 앱을 만들다 보면 이 설명만으로는 부족…
    
    → “**언제 실행되느냐**”보다 “**무엇을 기준으로 실행을 다시 하느냐**”, “**실행된 후에 어떤 결과가 남느냐**”를 정확히 이해하는 게 중요
    
- 컴포넌트가 렌더된 후, 브라우저가 실제로 화면을 그리고 난 후 실행됨
- DOM 위치 측정, 애니메이션을 주고 싶을 땐 useEffect가 늦음 → `useLayoutEffect` 써야 함
- 예) useEffect는 "페인트칠 끝나고 가구 배치하는 사람"
    - 벽이 마르기 전에 가구를 붙이면 안 되는 것처럼, DOM 작업엔 좀 더 빠른 useLayoutEffect 필요

---

## 의존성 배열 = useEffect 재실행 조건

- 배열이 비어 있으면 mount 시 1번만 실행됨
- 배열 안에 값이 있으면 그 값이 변경될 때마다 실행
- 생략하면 렌더링마다 실행 → 거의 안 씀

```jsx
useEffect(() => {
  console.log("count 바뀔 때마다");
}, [count]);

useEffect(() => {
  console.log("처음 한 번만");
}, []);
```

- 예) 이사 갈 때마다 주소 변경 알림을 우체국에 보내는 것
  - 안 보내면 계속 예전 주소로 택배 감

---

## 클로저로 인한 값 참조 이슈

- useEffect는 선언 시점의 값을 캡처해서 기억함
- 의존성 배열이 `[]`일 경우 첫 렌더 시의 값만 기억 → 이후 값 변화 반영 안 됨

```jsx
useEffect(() => {
  console.log(count); // 항상 초기 count 출력됨
}, []);
```

최신 값 쓰고 싶으면 의존성 배열에 반드시 넣어야 함

안 그러면 이전 값 참조하면서 버그 생김

---

## 비동기 작업 정리 필요 (클린업 함수)

- 데이터를 요청했는데, 사용자가 그 컴포넌트를 떠나버리면 어떻게 될까?
    
    ```jsx
    useEffect(() => {
      fetch("/data").then(res => res.json()).then(setData);
    }, []);
    ```
    
    이미 컴포넌트는 사라졌지만 이 코드는 페이지를 떠나도 `setData`가 실행됨…  ⇒ 메모리 누수, 경고 발생 가능
    

```jsx
useEffect(() => {
  const controller = new AbortController();

  fetch("/data", { signal: controller.signal })
    .then(res => res.json())
    .then(setData)
    .catch(err => {
      if (err.name === "AbortError") {
        console.log("요청 취소됨");
      }
    });

  return () => controller.abort(); // 컴포넌트 사라질 때 요청 취소
}, []);
```

- 컴포넌트가 사라졌는데 fetch가 끝나서 setData가 실행되면 에러 날 수 있음
- 과거엔 `let isCancelled = false` 같은 플래그 썼는데, 요즘은 `AbortController` 쓰는 게 표준

- 예) 누군가에게 택배 시켰는데 이사 가버린 경우, 받을 사람이 없으니까 택배를 미리 취소해야 함 → `AbortController`

---

## 무한 루프 문제

- useEffect 안에서 상태를 바꾸고, 그 상태를 의존성 배열에 넣으면 루프 발생

```jsx
useEffect(() => {
  setCount(count + 1);
}, [count]); // → 무한 루프
```

이유: count 바뀜 → effect 실행 → 또 count 바뀜 → 계속 반복

해결 방법:

- 조건문으로 제어하거나
- `useRef`를 써서 최초 실행 여부 체크

```jsx
const didMount = useRef(false);

useEffect(() => {
  if (didMount.current) {
    setCount(count + 1);
  } else {
    didMount.current = true;
  }
}, [count]);
```

## 요약

- useEffect는 브라우저가 화면 그린 뒤에 실행됨
- 의존성 배열이 재실행 기준 → 안 넣으면 최신 값 못 씀
- 비동기 작업은 정리 함수 꼭 필요 (AbortController 추천)
- 상태 변경 + 의존성 조합은 무한 루프 주의
- 최신 값 관리, 실행 조건, 클린업 포함한 전체 흐름 이해 필요
