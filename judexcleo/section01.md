# JavaScript 기초 정리

### HTML / CSS / JavaScript의 역할

-HTML: 구조 담당 (제목, 리스트 등) → 디자인 X
-CSS: 스타일링 (색상, 크기, 애니메이션 등)
-JavaScript: 웹 페이지에 동작 부여 (동적 처리)

---

### JavaScript 실행

브라우저 내 자바스크립트 엔진이 실행 (크롬, 엣지, 사파리 등)
console.log() : 콘솔에 출력
Live Server 열기 : html에서 cmd+shift+p -> Live Server
Live Server 끄기 : 하단 Port 왼쪽 종료 버튼

#### 변수 선언

```let age = 25;
console.log(age); // 25
```

상수(const)는 선언이후 값을 변경할 수 없기에 초기화 필요

#### 변수명 규칙

-숫자로 시작 X -특수문자 사용 X
-JS 키워드 사용 X

var vs let
var age = 25;
var age = 30; // 중복 선언 허용 X

let age = 25;
let age = 30; // 에러 발생
const (상수)
const age = 30;
age = 35; // 오류 발생

### 자료형

Primitive Type (원시타입): 하나의 값 (String, Number, Boolean, null, undefined)

- Number Type : 사칙연산 + 나머지 연산 가능
  - Infinity / -Infinity = 양/음의 무한대
  - NaN = Not a Number
- String Type : '' 이나 "" 사용해 묶기

```let myName = 'seunghee';
let myLocation = 'suwon';
let introduce = myName + myLocation;
let greeting = `Hello, ${name}`; //백틱 이용해 문장형성
```

- Boolean Type = true / false
- Null Type = 아무것도 담기지 않을 때
- Undefined Type : 초기화하지 않았을 때, 타입 모를 때

```let none;
console.log(none);
```

Non-Primitive Type : 객체, 배열 등

### 형 변환

묵시적 형 변환 : 직접 설정하지 않아도 알아서 js 엔진이 형을 변환

```let num = 10;
let str = "20";
const result = num + str; // 숫자를 문자열로 js가 변환
console.log(result); // 1020
```

명시적 형 변환 : 개발자가 함수 등을 이용해 형 변환을 일으킴

// 문자열 -> 숫자

```
let str1 = "12";
let strToNum1 = Number(str1);

let str2 = "12개"; //숫자가 앞쪽으로
let strToNum2 = parseInt(str2); // 10

// 숫자 -> 문자열
let num1 = 20;
let numToStr1 = String(num1);
```

### 연산자

let a = 10;
a += 5; // 복합 연산자, a에 5 더하기
a++; // 증감 연산자

```
console.log(a++); // 10 (후위) , a 출력후 +1해서 저장
console.log(++a); // 12 (전위) , a+1 출력하기
```

- 논리 연산자
  !true; // false
  true && false; // false
  true || false; // true
- 비교 연산자
  1 == "1"; // true (값만 비교)
  1 === "1"; // false (타입까지 비교)
- typeof 연산자 = 값의 타입을 문자열로 반환하는 기능
  let t1 = typeof var1; //
- Null 병합 연산자
  let a = undefined;
  let ab = a ?? 10; // undefined인 것을 저장

* 둘다 값이 있을 경우 앞에 있는 값으로 저장

- 상황 연산자 = 조건식을 이횽해서 참, 거짓일 때의 값을 다르게 변환

```let var8 = 10;
// 변수 res에 var8의 값이 짝수이면 "짝", 홀수이면 "홀"
let res = var8 %2 === 0? "짝수" : "홀수";
console.log(res);
```

### 조건문

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

### 함수

기본 함수
function getArea(width, height) {
return width \* height;
}
console.log(getArea(100, 200));
함수 표현식 & 화살표 함수
let helloA = function() {
return "안녕하세요";
};

const helloC = () => {
return "안녕하세요";
};

### 콜백 함수

function checkMood(mood, goodCallback, badCallback) {
if (mood == "good") goodCallback();
else badCallback();
}

### 객체

let person = {
name: "이정환",
age: 25
};

console.log("name" in person); // true
person.name = null;

### 배열

let arr = [1, "2", true, null];
arr.push({ key: "value" });
console.log(arr.length);

### 반복문

for (let i = 1; i <= 100; i++) {
console.log("winterlood");
}

const keys = Object.keys(person);

### 배열 내장 함수

const arr = [1, 2, 3, 4];

const doubled = arr.map(elm => elm \* 2);
arr.forEach(elm => console.log(elm));

arr.includes(3); // true
arr.indexOf(3); // 2

const idx = colors.findIndex(elm => elm.color === "blue");

arr.slice(0, 2);
arr.concat(colors);

numbers.sort((a, b) => a - b);
arrHi.join(" ");
