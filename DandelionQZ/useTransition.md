### useTransition이란?

useTransition은 UI 업데이트의 우선순위를 나누기 위해 사용하는 React 훅이다.

화면이 느려지는 작업을 분리해서, 사용자 입력과 같은 긴급한 업데이트가 먼저 반영되도록 한다.
<br></br>

### 사용법

```tsx
const [isPending, startTransition] = useTransition();
```

- `isPending`: 전환 중일 때 `true`. 로딩 스피너 등을 표시할 때 사용.
- `startTransition(callback)`: 이 안에 넣은 상태 업데이트는 "덜 중요한 작업" 으로 처리됨.
<br></br>

### 왜 써야 할까?

- 문제 상황
    
    ```tsx
    // 검색창에 입력할 때마다 긴 리스트를 필터링
    // ㅇ -> 아 -> 안 -> 안ㄱ -> 안겨 -> 안경 
    setInputValue(input);
    setFilteredItems(expensiveFilter(input)); //무거운 연산이라 입력 반응이 느려짐
    ```
    
- 해결
    
    ```tsx
    setInputValue(input); // 즉시 반영됨
    
    startTransition(() => {
      setFilteredItems(expensiveFilter(input)); // 나중에 처리됨 (react가 적절한 시점에 처리함)
    });
    ```
<br></br>
    

### 사용 예시

```tsx
import React, { useState, useTransition, ChangeEvent } from 'react';

const SearchComponent = () => {
  const [inputValue, setInputValue] = useState<string>('');
  const [filtered, setFiltered] = useState<string[]>([]);
  const [isPending, startTransition] = useTransition();

  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    setInputValue(value); // 우선 input의 값을 업데이트 

    startTransition(() => { // 후순위 업데이트 (모든 입력마다 실행됨) 
	    fetch(`/api/items?q=${value}`)
        .then(res => res.json())
        .then((data: string[]) => {
          setFiltered(data);
        });
    });
  }

  return (
    <div>
      <input type="text" value={inputValue} onChange={handleChange} />
      {isPending && <p>Loading...</p>}
      <ul>
        {filtered.map(item => <li key={item}>{item}</li>)}
      </ul>
    </div>
  );
}

```
<br></br>

### Debounce와의 차이점

| 항목 | startTransition | Debouncing |
| --- | --- | --- |
| **목적** | 긴급한 업데이트(예: 입력값)는 먼저, 무거운 작업은 나중 | 너무 많은 호출을 지연 또는 제한 |
| **작동 방식** | 우선순위 기반 업데이트 (React의 동시성 처리 기능) | 일정 시간 동안 입력이 멈춰야 실행됨 |
| **실행 횟수** | 모든 입력마다 실행됨 | 지연 시간 내에는 한 번만 실행됨 |
| **네트워크 요청 제어** | 직접 제어 안 됨 (많이 요청될 수 있음) | 요청 횟수 자체를 줄여줌 |
| **예시** | UI는 빠르게 반영되지만, 연산은 나중에 처리 | 300ms 동안 입력 없을 때만 API 호출 |

<br></br>

### 사용 예시 (useTransition + Debounce)

```tsx
import { useState, useTransition, ChangeEvent, useEffect } from 'react';
import { debounce } from 'lodash';

const SearchComponent = () => {
  const [inputValue, setInputValue] = useState('');
  const [filtered, setFiltered] = useState<string[]>([]);
  const [isPending, startTransition] = useTransition();

  // 디바운스된 검색 함수
  const debouncedSearch = debounce((query: string) => {
    startTransition(() => {
      fetch(`/api/items?q=${query}`)
        .then(res => res.json())
        .then((data: string[]) => {
          setFiltered(data);
        });
    });
  }, 300);

  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value;
    setInputValue(value);
    debouncedSearch(value); // 디바운스된 함수 호출
  };

  return (
    <>
      <input type="text" value={inputValue} onChange={handleChange} />
      {isPending && <p>Loading...</p>}
      <ul>{filtered.map(item => <li key={item}>{item}</li>)}</ul>
    </>
  );
};

```
<br></br>

### 언제 써야 하나?

- 무거운 렌더링, 계산이 있을 때
- 사용자 입력 반응성을 높이고 싶을 때
- 단순한 상태 변경에는 필요 없음
<br></br>

### 정리

| 항목 | 설명 |
| --- | --- |
| 목적 | 느린 업데이트를 우선순위 낮춰 사용자 경험 개선 |
| 반환값 | [isPending, startTransition] |
| isPending | transition 중인지 여부 표시 |
| startTransition() | 긴급하지 않은 업데이트 감싸기 |
| 비슷한 개념 | useDeferredValue (입력값 자체를 지연시킴) |

<br></br>

### useDeferredValue

| 항목 | useDeferredValue | debounce |
| --- | --- | --- |
| **기능** | 리액트의 값 업데이트를 "지연" | 함수 실행 자체를 "지연 또는 제한" |
| **제공 주체** | React (내장 Hook) | 직접 구현 or 라이브러리 (lodash, use-debounce) |
| **지연 기준** | React의 스케줄링 우선순위 기반 | 명시적인 시간 지연 (예: 300ms) |
| **실행 횟수 제어** | **안됨 (모든 입력 처리됨)** | 됨 (지정 시간 내 한 번만 실행) |
| **주용도** | 무거운 렌더링이 생기는 경우 | 빈번한 API 호출, 연산 최적화 |
| **입력 지연 여부** | **입력은 즉시 반영됨** | 입력 처리를 시간 기준으로 지연 |

```tsx
const [input, setInput] = useState('');
const deferredInput = useDeferredValue(input); // 느린 버전의 input

const filtered = useMemo(() => {
  return items.filter(item => item.includes(deferredInput));
}, [deferredInput]); // deferredInput이 바뀌면 다시 실행됨 
```

- input이 바뀔 때마다 결국 deferredInput도 업데이트됨 (렌더링 우선순위만 낮출 뿐)
- useDeferredValue → 렌더링 성능 최적화가 필요할 때 사용
<br></br>

### 참고 링크

[리액트 useTransition 공식문서](https://react.dev/community/translations)
