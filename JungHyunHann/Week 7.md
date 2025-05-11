## React 상태 관리의 스펙트럼

### 1. useState – 가장 단순하고 빠른 상태 관리

```jsx
const [count, setCount] = useState(0);
```

- **언제 사용하나?**
    
    → 간단하고 단순한 상태 관리 (예: 모달 열기/닫기, 탭 선택)
    
- **장점**
    - 쉽고 직관적
    - 빠른 개발 가능
- **한계**
    - 여러 상태가 얽히면 복잡해짐
    - 관련 상태끼리 로직이 흩어짐 (관심사 분리 어려움)

---

### 2. useReducer – 상태 전이(State Transition) 로직이 명확한 구조

```jsx
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
```

- **언제 사용하나?**
    
    → 여러 개의 상태가 서로 영향을 주거나, 조건에 따라 다르게 변화할 때
    
- **장점**
    - 상태 변화가 액션 기반이라 예측/디버깅 쉬움
    - 테스트가 용이하고 코드 일관성이 좋음
- **한계**
    - 상태 공유에는 적합하지 않음
    - 상위 컴포넌트에 올려서 내려줘야 함 → props drilling 발생 가능

---

### 3. Context API – 컴포넌트 트리를 넘는 상태 공유

```jsx
const UserContext = createContext();
...
<UserContext.Provider value={user}>
  <SomeComponent />
</UserContext.Provider>
```

- **언제 사용하나?**
    
    → 로그인 정보, 테마, 언어, 글로벌 설정 값 등 여러 컴포넌트가 필요로 하는 값
    
- **장점**
    - props drilling 없이 전역처럼 상태 공유 가능
    - `useContext`로 쉽게 접근 가능
- **한계**
    - 큰 데이터를 하나에 몰아 넣으면 리렌더링 최적화 어려움
    - 상태가 복잡하면 Context 분리/최적화 필요

---

### 4. 전역 상태 관리 라이브러리 (Redux, Zustand, Recoil 등)

| 라이브러리 | 특징 |
| --- | --- |
| Redux | 미들웨어 및 DevTools가 강력하고 구조화에 좋음 |
| Zustand | 매우 간단한 API, 최소한의 보일러플레이트 |
| Recoil | 비동기/복잡한 상태 그래프에 적합함 |
- **언제 사용하나?**
    
    → 페이지 전반의 상태를 공유하거나, 실시간 데이터/비동기 캐싱 필요할 때
    
- **장점**
    - 구조적이며 확장 가능
    - 대규모 앱에서 유지보수 용이
- **단점**
    - 러닝 커브 존재
    - 간단한 앱엔 과도할 수 있음

---

### 1️⃣ 문제 상황

- 규모가 커지면 `useState`만으로 관리하기 어려움
    
    → 상태 간 의존성 증가
    
    → 조건별 로직 분산
    
- Context만 쓰면 상태가 뒤섞이고 최적화 어려움
- Redux는 너무 무거움: action type 정의, store 구성 등 보일러플레이트가 큼

---

### 2️⃣ 해결 구조: `useReducer` + `Context`

```jsx
const AppContext = createContext();
const [state, dispatch] = useReducer(reducer, initialState);

<AppContext.Provider value={{ state, dispatch }}>
  <App />
</AppContext.Provider>
```

- **Reducer로**: 복잡한 로직 정리, 상태 전이 명확
- **Context로**: 필요한 컴포넌트에 상태 및 dispatch 공유

---

### 3️⃣ 예시: 블로그 글쓰기 기능

| 상태 | 처리 방식 |
| --- | --- |
| 제목/내용 입력값 | useReducer |
| 로그인된 유저 정보 | Context |
| 작성 완료 여부 | reducer 액션으로 상태 업데이트 |

→ 복잡한 폼 로직은 reducer로 정리

→ 로그인 정보는 전역 Context로 제공

→ 컴포넌트마다 필요한 정보만 받아서 사용
