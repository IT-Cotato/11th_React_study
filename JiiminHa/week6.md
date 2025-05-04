# UUID란?

UUID(Universally Unique Identifier)는 **전 세계적으로 고유한 식별자**를 만들기 위한 표준입니다. 흔히 128비트(16바이트) 길이로 구성되며, 예를 들어 다음과 같은 형식입니다:

```
550e8400-e29b-41d4-a716-446655440000
```

---

## UUID의 주요 특징

* **고유성**: 중복될 가능성이 매우 낮음
* **포맷**: 하이픈으로 구분된 5개의 그룹 (8-4-4-4-12 문자)
* **버전**:

  * v1: 타임스탬프 기반
  * v4: 랜덤 기반 (가장 일반적)

---

## JavaScript에서 UUID 생성 방법

### 1. `uuid` 라이브러리 설치

```bash
npm install uuid
```

### 2. 사용 예시

```js
import { v4 as uuidv4 } from 'uuid';

const id = uuidv4();
console.log(id); // "110ec58a-a0f2-4ac4-8393-c866d813b8d1"
```

---

## React에서 UUID 사용 사례

### ✅ 적절한 사용 예시

* 사용자가 작성한 임시 항목에 고유 ID 부여
* 비동기적으로 생성된 데이터 구분
* DB에 저장되기 전 프론트엔드에서 식별자 필요할 때

```js
const tempItems = data.map(item => ({ ...item, tempId: uuidv4() }));
```

---

## React에서 key로 UUID를 쓰면 안 되는 이유

```jsx
// ❌ 비추 예시
{items.map(item => (
  <Component key={uuidv4()} data={item} />
))}
```

### ❗ 문제점

* **매 렌더링마다 UUID가 새로 생성됨 → React는 매번 컴포넌트를 새로 렌더링**
* 이는 **성능 저하와 불필요한 리렌더링**을 유발함

### 🔑 좋은 key 사용 기준

* 고정된 값이어야 함 (id, index 등)
* 항목이 리스트 내에서 변하지 않는다는 전제하에 index도 사용 가능

```jsx
// ✅ 추천 예시
{items.map(item => (
  <Component key={item.id} data={item} />
))}
```

---

## 요약

| 항목             | 내용                       |
| -------------- | ------------------------ |
| UUID 사용 목적     | 전역 고유 식별자 생성             |
| React에서 적절한 용도 | 데이터 구분, DB 입력 전 임시 ID 부여 |
| key로의 사용       | ❌ 비추천 (성능 문제)            |

---

## 참고

* [https://www.npmjs.com/package/uuid](https://www.npmjs.com/package/uuid)
* [https://reactjs.org/docs/lists-and-keys.html](https://reactjs.org/docs/lists-and-keys.html)
