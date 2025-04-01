# JavaScript 기초 정리

### HTML / CSS / JavaScript의 역할

-HTML: 구조 담당 (제목, 리스트 등) → 디자인 X
-CSS: 스타일링 (색상, 크기, 애니메이션 등)
-JavaScript: 웹 페이지에 동작 부여 (동적 처리)

---

### JavaScript 실행

브라우저 내 자바스크립트 엔진이 실행 (크롬, 엣지, 사파리 등)
console.log() : 콘솔에 출력

#### 변수 선언

let age = 25;
console.log(age); // 25

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

Primitive Type: 하나의 값 (String, Number, Boolean, null, undefined)
Non-Primitive Type: 객체, 배열 등
문자열 (Template Literal)
let name = "이정환";
let greeting = `Hello, ${name}`;
Boolean, Null, Undefined
let isSwitchOff = false;

let a;
console.log(a); // undefined
a = null;
console.log(a); // null
형 변환

let numberA = 12;
let numberB = "2";

console.log(numberA \* numberB); // 24 (묵시적 형변환)
console.log(numberA + numberB); // "122"
console.log(numberA + parseInt(numberB)); // 14 (명시적 형변환) ####연산자

let a = 10;
a += 5; // 복합 연산자

a++; // 증감 연산자
console.log(a++); // 10 (후위)
console.log(++a); // 12 (전위)
논리 연산자
!true; // false
true && false; // false
true || false; // true
비교 연산자
1 == "1"; // true (값만 비교)
1 === "1"; // false (타입까지 비교)

typeof "1"; // string
Null 병합 연산자
let a = undefined;
a = a ?? 10; // 10

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
