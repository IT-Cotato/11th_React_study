# JavaScript 심화 정리

### Truthy와 Falsy

> 참이나 거짓을 의미하지 않는 값도, 조건문 내에서 참이나 거짓으로 평가하는 특징

- 자바스크립트의 모든 값은 truthy 혹은 falsy 하다.

#### Falsy한 값

```
let f1 = undefined;
let f2 = null;
let f3 = 0;
let f4 = -0;
let f5 = NaN;
let f6 = "";
let f7 = 0n;
```

#### Truthy한 값

** 7가지 falsy한 값을 제외한 나머지 모든 값 **

```
let t1 = "hell0";
let t2 = 123;
let t3 = [];
let t4 = {};
let t5 = () => ();
```

#### 활용사례

```
fuction printName(person){
if (!person){
    console.log("there's no value in Person");
    return;
}
console.log(person.name);
}
let person ={ name : "seunghee"};
printName(person);
```

- truthy/falsy를 쓰지 않으면 person 값이 undefined/null/.. 등일 때의 실행을 각각 만들어주어야 하지만, 이를 쓰면 간편하게 나타낼 수 있다.

### 단락평가 (Short-circuit Evaluation)

> and 와 or 등의 구문에서 첫 번째 값만으로 결과를 판단할 수 있다면 두 번째 값에는 접근하지 않는 성질

```
function returnFalse(){
    console.log("False function");
    return false;
}

fuction returnTrue(){
    console.log("True function");
    return true;
}

console.log(returnFalse() && returnTrue()); // returnTrue()가 호출조차 되지 않음

// 이의 결과로 "False function" false 가 출력되었다.

console.log(returnTrue() || returnFalse()); // returnFalse()가 호출조차 되지 않음
```

true/false 뿐 아니라 truthy/falsy 한 값도 출력

#### 단락평가 활용 사례

```
fuction printName(person){
const name = person&&person.name
console.log(name || "person의 값이 없음");
}
let person ={ name : "seunghee"};
printName(person);
```

### 구조분해할당

#### 배열의 구조분해할당

```
let arr [1,2,3];
let [one,two,three,four = 4] = arr; // 일일이 할당할 필요 없이 한번에 할당 가능 , 4는 기본값
console.log(one,two,three);

```

#### 객체의 구조분해 할당

```
let person = {
    name: "seunghee";,
    age: 20;,
    hobby : "games";,
}
let {name, age : myAge, hobby, extra = "hello"} = person;
// person의 property 값을 위 변수들에 할당
console.log(name, myAge, hobby, extra);
```

#### 객체의 구조분해 할당을 이용해 함수의 매개변수를 받는 방법

```
const func = ({name, age, hobby, extra}) => {
    console.log(name, age, hobby, extra);
};
func(person);
```

### Spread 연산자와 Rest 매개변수

#### Spread 연산자

객체나 배열에 저장된 여러 개의 값을 개별로 흩뿌려주는 역할

```
let arr1 = [1,2,3];
let arr2 = [4,...arr1,5,6]; // [4,1,2,3,5,6] 이렇게 흩뿌리고 싶을때

let obj1 ={
    a: 1,
    b: 2,
};
let obj2 ={
    ...obj1,
    c: 3,
    d: 4,
};

function funcA(p1,p2,p3){

}
funcA(..arr1);
// arr1의 값들을 매개변수로 받아 funcA의 매개변수로 1,2,3 이 들어온 것으로 작동
```

#### Rest 매개변수

: 나머지 매개변수

```
function funcB(one,...rest){
    console.log(rest);
}
funcB(...arr1);
// spread 연산자로 funcB의 매개변수를 arr1로 불러온다음,
배열의 첫번째 값을 one에 저장하고 나머지는 rest 변수에 배열 형태로 저장하였다.
```

### 원시타입 vs 객체타입

원시타입 : 값 자체로서 변수에 저장되고 복사된다 = 불변값
객체타입 : 참조값을 통해 변수에 저장되고 복사된다 = 가변값

- `원시 타입` : 변수의 값을 1에서 2로 변경을 하게 되더라도 메모리 공간에
  저장되어 있었던 값은 실제로 수정되지 않음
  대신에 변경해야 할 값을 새로운 메모리 공간에 추가적으로 저장하고 이어서 변수가 가리키던 주소 공간을 추가된 주소 공간을 가리키게 함

- `객체 타입` : 새로운 객체를 선언해서 이전 객체에 할당하면, 새로운 객체는 이전 객체 변수가 가리키고 있던 참조 값을 똑같이 가리키게 됨
  (원시타입처럼 메모리 공간에 데이터를 추가적으로 생성하지 않음) 이때 새로운 객체에 프로퍼티를 변경하는 코드를 작성하면 가리키고 있던 이전 객체의 원본 데이터를 변경한다. 즉, 새로운 객체만 변경되는 것이 아니라 이전 객체도 함께 바뀌어 버림

| 타입      | 값의 형태 | 메모리 공간          | 수정 시 변화          |
| --------- | --------- | -------------------- | --------------------- |
| 원시 타입 | 불변값    | 원본 데이터          | 추가 메모리 공간 확보 |
| 객체 타입 | 가변값    | 참조값과 원본 데이터 | 기존 메모리 공간 활용 |

#### 객체 타입 주의사항

(1) 의도치 않게 값이 수정될 수 있음 (Side Effect)

- 다른 객체의 참조값을 가리키는 객체를 수정했을 때, 해당 객체 및 다른 객체 모두 의도하지 않게 모두 수정하거나 아니면 그 사실 자체를 모르고 있을 경우에 꽤나 큰 오류가 발생할 수 있음
- 객체의 값을 복사할 때는 그냥 대입 연산자로 변수의 참조 값을 복사하도록 하는 게 아니라 새로운 객체 리터럴을 생성하고 그 내부에 스프레드 연산자 등을 이용해 내부 프로퍼티만 따로 복사해오는 방식으로 객체를 복사해야 함.
  -> 아예 새로운 객체를 생성해서 초기화하는 것으로 평가되어 새로운 참조값에 새로운 객체 데이터가 따로 저장됨

얕은 복사 : 객체의 참조값을 그대로 대입 연산자를 통해서 복사하는 방식
깊은 복사 : 새로운 객체를 생성하면서 내부 프로퍼티만 따로 복사해주는 방식

(2) 객체간의 비교는 기본적으로 참조값을 기준으로 이루어진다.

- o2는 얕은 복사로 o1과 같은 참조값을 갖으며 결국 같은 객체를 가리킴.
- 반면 o3는 깊은 복사로 객체를 생성하면서 spread 연산자를 이용해서 o1의 객체의 프로퍼티만 복사함. 그 결과로 메모리 상에서 새로운 객체가 하나 생성이 되고, 새로운 참조값으로 데이터를 가리키는 방식으로 메모리에 저장됨.
- o1과 o2를 일치 연산자(===)로 비교를 하면 결괏값으로 true
  현재 같은 구조의 객체를 보관하고 있을 뿐만 아니라 참조값까지 동일하기 때문에 당연히 서로 같은 값으로 평가됨.
- 그리고 o1과 o3를 일치 연산자로 비교하면 결과값은 false
  name 프로퍼티를 가지는 o1과 o3는 같은 객체로 볼 수 있으나 이 객체 간의 비교 연산은 기본적으로 참조값을 기준으로 이루어지기 때문에 참조값이 서로 다른 두 객체는 다르다

- 참조값이 아닌 프로퍼티를 기준으로 두 객체를 비교하고 싶다면, JSON.stringify() 같은 객체를 문자열로 형변환하는 내장함수를 이용해서 참조값이 아닌 프로퍼티를 기준으로 비교하도록 설정해 주어야 함

- 참조값을 기준으로 비교하는 방식을 얕은 비교, **JSON.stringify()**와 같이 객체를 문자열로 변환하여 내부 프로퍼티를 기준으로 비교하는 방식을 깊은 비교
  ![](https://velog.velcdn.com/images/tmdgml110806/post/e4a9e252-65c8-44a8-abfd-76579a5c4801/image.png)

### 반복문으로 배열과 객체 순회하기

> 순회(Iteration) : 배열이나 객체에 저장된 여러 개의 값에 순서대로 하나씩 접근하는 것
> 배열 순회 : 인덱스를 기준으로 순서대로 값에 접근하는 것
> 객체 순회 : 객체의 프로퍼티 key 또는 value를 기준으로 접근하는 것

#### 1. 배열 인덱스

- 배열이나 함수도 객체이기에 프로퍼티 또는 메서드를 가질 수 있음

```
// 1. 배열 순회
let arr = [1,2,3];

// 1.1 배열 인덱스
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}

let arr2 = [4, 5, 6, 7, 8];
for (let i = 0; i < arr2.length; i++) {
    console.log(arr2[i]);
}
```

#### 2. for of 반복문

- for of 반복문은 of 뒤에 오는 배열의 값을 하나씩 순서대로 꺼내서 카운터 변수에 저장함
- 배열의 인덱스를 이용하는 방식 : 카운터 변수에 배열의 인덱스가 저장되기 때문에 for 문 안에서 인덱스를 통한 작업을 할 수 있음
- for of 반복문 : 인덱스를 저장하지 않고 그냥 배열에 있는 값들을 순서대로 순회만 함

```
// 1.2 for of 반복문
for (let item of arr) {
    console.log(item);
}
```

#### 3. Object.keys() 내장함수

- 인수로 주어진 객체에서 key 값들만 뽑아서 새로운 배열로 반환
- Object.keys() 내장함수를 통해 반환된 새로운 배열을 통해 for 문는 또는 for of 반복문으로 순회할 수 있음
- 추가로 key 값과 함께 객체의 value 값도 동시에 순회하고 싶다면 대괄호 표기법을 사용

```
// 2.1 Object.keys 사용
// -> 객체에서 key 값들만 뽑아서 새로운 배열로 반환
let keys = Object.keys(person);
console.log(keys);

// for (let i=0; i < keys.length; i++) {
//     console.log(keys[i]);
// }

for (let key of keys) {
    const value = person[key];
    console.log(key, value);
}
```

#### 4. Object.values() 내장함수

- 인수로 주어진 객체에서 value 값들만 뽑아서 새로운 배열로 반환
- 첫번째 방식과 동일하게 반환된 배열을 통해 for문 또는 for of 반복문으로 순회를
- 만약 객체를 순회할 때, value 값들만 순회하면 된다면 Object.values() 내장함수를 이용하는 방식을 사용

```
// 2.2 Object.values
// -> 객체에서 value 값들만 뽑아서 새로운 배열로 반환
let values = Object.values(person);
console.log(values);

for (let value of valeus) {
    console.log(value);
}
```

#### 5. for in 반복문

- for in 반복문은 객체만을 위해 존재하는 특수한 반복문
- 사용법은 for of 반복문의 사용법과 비슷함 (in 뒤에 있는 객체의 프로퍼티 key를 순서대로 카운터 변수에 할당)
- Object.keys() 또는 Object.values() 내장함수를 사용하지 않고도 객체를 순회할 수 있음
- _주의_ for of 반복문과 for in 반복문을 헷갈리면 안됨
  for of 반복문은 배열에만 쓸 수 있고, for in 반복문은 객체에만 쓸 수 있움
  객체 프로퍼티를 검사할 때 in 연산자를 사용했던 것을 떠올리면 됨

```
// 2.3 for in 반복문
for (let key in person) {
    const value = person[key];
    console.log(key, value);
}

// chapter15 - 프로퍼티의 존재 유무를 확인하는 방법 (in 연산자)
console.log('name' in person);
console.log('hobby' in person);
```
