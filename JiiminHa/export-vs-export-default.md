# `export default` vs `export` – React에서의 차이

React에서 컴포넌트를 작성하고 내보낼 때, `export default`와 `export` 두 가지 방식이 사용됩니다. 이 둘은 **컴포넌트를 다른 파일에서 어떻게 import(가져오기)** 하느냐에 따라 중요한 차이가 있습니다.

---

## ✅ export default (기본 내보내기)

- **한 파일에 하나만** 사용할 수 있음
- import 시 **이름을 자유롭게** 지정 가능

### 예시 1 – 기본 내보내기

```jsx
// Button.js
export default function Button() {
  return <button>Click me</button>;
}
```

```jsx
// App.js
import MyButton from "./Button"; // 이름을 자유롭게 지정 가능
```

---

## ✅ export (이름 있는 내보내기, named export)

- **한 파일에서 여러 개 내보내기 가능**
- import 시 **이름을 정확히 일치**시켜야 함 (또는 별칭 사용)

### 예시 2 – 이름 있는 내보내기

```jsx
// components.js
export function Header() {
  return <h1>Header</h1>;
}

export function Footer() {
  return <footer>Footer</footer>;
}
```

```jsx
// App.js
import { Header, Footer } from "./components";
```

혹은 이름이 길거나 바꾸고 싶을 경우:

```jsx
import { Header as PageHeader } from "./components";
```

---

## 📌 정리

| 구분        | export default           | export (named)           |
| ----------- | ------------------------ | ------------------------ |
| 개수        | 파일당 1개               | 여러 개 가능             |
| import 이름 | 자유롭게 지정 가능       | 반드시 이름 일치 or 별칭 |
| 사용 예시   | 주요 컴포넌트, 기본 유틸 | 여러 유틸, 여러 컴포넌트 |

---

## 🎯 React에서 언제 어떤 걸 쓰면 좋을까?

- `export default`: 페이지 단위 컴포넌트, 재사용성이 높은 주 컴포넌트
- `export`: 여러 개의 유틸 함수, 훅(hook), 작은 컴포넌트를 한 파일에 정의할 때

---

## 🧠 팁

> 하나의 컴포넌트만 있는 파일이라면 `export default`를 사용하는 게 일반적입니다.  
> 여러 컴포넌트를 묶어서 관리하려면 `named export`를 고려해보세요.

---

📚 참고:

- [React 공식 문서 – 코드 분할과 모듈화](https://reactjs.org/docs/code-splitting.html)
- [MDN – export](https://developer.mozilla.org/ko/docs/web/javascript/reference/statements/export)
