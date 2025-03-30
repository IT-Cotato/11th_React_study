### JavaScript 기초 정리

---

### HTML / CSS / JavaScript의 역할

- **HTML**: 구조 담당 (제목, 리스트 등)
  - 디자인은 불가능함
- **CSS**: 스타일링 (색상, 크기, 애니메이션 등)
- **JavaScript**: 웹 페이지에 동작 부여 (동적 처리)

---

### JavaScript 실행

- 자바스크립트 엔진이 웹 브라우저에 포함되어 있음
- `console.log()` : 괄호 안의 값을 콘솔에 출력

---

### 변수 선언

```js
let age = 25;
console.log(age); // 25
```

- 변수명은 숫자로 시작할 수 없음
- 특수문자, 키워드 사용 금지

#### var vs let

```js
var age = 25;
var age = 30; // 중복 허용됨 (지양)

let age = 25;
let age = 30; // 에러 발생
```

#### const (상수)

```js
const age = 30;
age = 35; // 오류 발생
```

---

### 자료형

- **Primitive Type**: 하나의 값만 저장 (String, Number, Boolean, null, undefined)
- **Non-Primitive Type**: 여러 값 저장 가능 (객체, 배열 등)

#### 문자열 (Template Literal)

```js
let name = "이정환";
let greeting = `Hello, ${name}`;
```

#### Boolean, Null, Undefined

```js
let a;
console.log(a); // undefined

a = null;
console.log(a); // null
```

---

### 형 변환

```js
let numberA = 12;
let numberB = "2";

console.log(numberA * numberB); // 24 (묵시적 형변환)
console.log(numberA + numberB); // "122"
console.log(numberA + parseInt(numberB)); // 14 (명시적 형변환)
```

---

### 연산자

- `=`, `+=`, `++`, `--` 등

```js
let a = 10;
a++;
console.log(a); // 11
console.log(a++); // 11 → 이후에 12
console.log(++a); // 13
```

#### 논리 연산자

```js
!true; // false
true && false; // false
true || false; // true
```

#### 비교 연산자

```js
1 == "1"; // true (값만 비교)
1 === "1"; // false (타입까지 비교)
typeof "1"; // string
```

#### Null 병합 연산자

```js
let a;
a = a ?? 10; // 10
```

---

### 조건문

```js
if (a >= 5) {
  console.log("5 이상");
} else {
  console.log("5 이하");
}

switch (country) {
  case "ko":
    console.log("한국");
    break;
  default:
    console.log("미분류");
}
```

---

### 함수

#### 함수 선언식

```js
function getArea(width, height) {
  return width * height;
}
console.log(getArea(100, 200));
```

#### 함수 표현식 / 화살표 함수

```js
let helloA = function () {
  return "안녕하세요";
};

const helloC = () => {
  return "안녕하세요";
};
```

---

### 콜백 함수

```js
function checkMood(mood, goodCallback, badCallback) {
  if (mood == "good") goodCallback();
  else badCallback();
}
```

---

### 객체

```js
let person = {
  name: "이정환",
  age: 25,
};

console.log("name" in person); // true
person.name = null;
```

---

### 배열

```js
let arr = [1, "2", true, null];
arr.push({ key: "value" });
console.log(arr.length);
```

---

### 반복문

```js
for (let i = 1; i <= 100; i++) {
  console.log("winterlood");
}

const keys = Object.keys(person);
```

---

### 배열 내장 함수

```js
const arr = [1, 2, 3, 4];

const doubled = arr.map((elm) => elm * 2);
arr.forEach((elm) => console.log(elm));

arr.includes(3); // true
arr.indexOf(3); // 2

const idx = colors.findIndex((elm) => elm.color === "blue");

arr.slice(0, 2);
arr.concat(colors);

let numbers = [0, 1, 3, 2, 7, 6, 10];
numbers.sort((a, b) => a - b);

const arrHi = ["이정환님", "안녕하세요"];
console.log(arrHi.join(" "));
```

---
