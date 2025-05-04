# 리액트 입문 정리 Section 5

## 컴포넌트란?

UI를 구성하는 독립적이고 재사용 가능한 코드 블록
한 번 만들면 여러 곳에서 사용할 수 있어 코드 중복을 줄이고 유지보수가 쉬워진다

### 컴포넌트 종류

* **함수형 컴포넌트**: JS 함수처럼 정의 , React Hooks로 생명주기 관리 가능하며 간결하고 직관적

  ```jsx
  const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;

  function Header() {
    return (
      <header>
        <h1>header</h1>
      </header>
    );
  }

  function App() {
    return (
      <>
        <Header />
        <h1>안녕 리액트</h1>
      </>
    );
  }

  export default App;
  ```

* 보통 컴포넌트는 각각 파일로 분리하여 관리한다

### Root, 부모·자식 컴포넌트

* **App**: 리액트 앱의 Root이자 가장 상위 컴포넌트
* **Header**: App 내부에서 사용하는 자식 컴포넌트
* **부모 컴포넌트**: 자식 컴포넌트를 포함함
* **자식 컴포넌트**: 부모에게 호출되어 렌더링되며, 필요시 props로 데이터 받음

---

## JSX

JS 문법 확장. HTML처럼 보이는 문법으로 UI 구조를 작성한다

### 특징

1. HTML과 유사하지만, 실행 전에 JS로 변환됨

   ```jsx
   const element = <h1>Hello</h1>;
   // => React.createElement("h1", null, "Hello");
   ```

2. JS 표현식을 중괄호 `{}`로 삽입 가능

   ```jsx
   const name = "React";
   const element = <h1>Welcome to {name}!</h1>;
   ```

### 주요 규칙

1. 반드시 하나의 부모 요소로 감싸야 함

   ```jsx
   // 잘못된 예
   return (
     <h1>Header</h1>
     <p>Text</p>
   );

   // 올바른 예
   return (
     <div>
       <h1>Header</h1>
       <p>Text</p>
     </div>
   );
   ```

2. `class` 대신 `className` 사용 

3. self-closing 태그 필수 ... <img, src, a, br..>

   ```jsx
   <img src="image.jpg" alt="img" />
   ```

4. 중괄호 안에는 JS 표현식만 사용 가능 (숫자, 문자열, 배열 등)

---

## State로 사용자 입력 관리
![](https://velog.velcdn.com/images/tmdgml110806/post/627a5c60-9345-41eb-8566-25dcff0a07b3/image.png)

### 기본 방식

```jsx
const [name, setName] = useState("이름");
// 필드별 useState 사용
```

### 개선 방식

* 객체로 상태 통합
* onChange 함수 하나로 통일

```jsx
const [input, setInput] = useState({
  name: "", birth: "", country: "", bio: ""
});

const onChange = (e) => {
  setInput({
    ...input,
    [e.target.name]: e.target.value
  });
};
```

---

## useRef - 컴포넌트 변수 생성

### 용도

* DOM 요소 직접 참조
* 리렌더링 없이 값 저장

### 특징 비교

| 항목           | useState      | useRef         |
| ------------ | ------------- | -------------- |
| 리렌더링 여부      | 값 변경 시 리렌더링   | 값 변경해도 리렌더링 없음 |
| 목적           | 상태 관리 및 UI 갱신 | 값 저장, DOM 조작   |
| 초기화 시점       | 리렌더링마다 초기화    | 마운트 시 1회 초기화   |
| DOM 접근 가능 여부 | 불가능           | 가능             |

### 예시: 리렌더링 없이 값 증가

```jsx
const refObj = useRef(0);
console.log("Register 렌더링");

<button onClick={() => {
  refObj.current++;
  console.log(refObj.current);
}}>++</button>
```

### 예시: DOM 제어

```jsx
const inputRef = useRef();

const onSubmit = () => {
  if (input.name === "") {
    inputRef.current.focus(); // 포커스 이동
  }
};
```

### 일반 변수 vs useRef

* 일반 변수는 렌더링마다 초기화됨
* useRef는 값이 유지됨

---

## React Hooks

### 정의

함수형 컴포넌트에서 state와 lifecycle 기능을 쓸 수 있도록 해주는 기능 (v16.8부터)

### 특징

* 클래스 컴포넌트 없이도 상태관리 가능
* 순수 함수 기반 작성 가능
* 커스텀 훅으로 로직 재사용 가능
* 훅 호출 순서 보장됨

### 사용 규칙

* 최상위에서만 호출 (조건문/반복문 내부 X)
* 함수형 컴포넌트에서만 사용
* `use`로 시작하는 커스텀 훅 생성 가능

