### 동기 방식 처리

스레드에 추가된 태스크를 하나씩 수행하는 방식
스레드에 작업이 수행되고 있을때 다른 작업을 동시에 수행할 수 없다. → 블로킹 방식

#### 자바스크립트의 싱글 스레드 작업 방식

![](https://velog.velcdn.com/images/tmdgml110806/post/6da304b2-7f70-4f87-8f9a-57afe50efd1e/image.png)


자바스크립트는 싱글 스레드 작업 방식을 수행하며 동기적으로 작업을 수행한다. 이때, 특정 작업이 시간이 오래 걸리는 경우, 그 작업이 끝날때까지는 다음 작업을 수행할 수 없다. 전반적으로 작업 수행 시간이 길어지고 성능상 문제가 발생하게 된다.

### 비동기 방식 처리

![](https://velog.velcdn.com/images/tmdgml110806/post/b6241cdc-c1a1-47e5-8a22-55edc2fc84aa/image.png)


* 여러 개의 작업을 동시에 실행
 * 먼저 작성된 코드의 결과를 기다리지 않고 바로 다음 코드를 실행한다.
* 하나의 작업이 스레드를 점유하지 않는다 → 논블로킹 방식

#### setTimeout 함수를 이용한 비동기 함수 예시

자바스크립트는 싱글쓰레드 방식으로 동기적으로 작업을 수행한다.

``` 
function taskA(){
  console.log('A 작업 끝');
}

taskA();
console.log('코드 끝');
```

위의 코드를 실행하면 taskA 함수의 'A 작업 끝' 문자열이가 먼저 콘솔에 출력되고 이후 '코드 끝'이 순차적으로 콘솔에 출력되어 동기적으로 실행됨을 알 수 있다.

taskA 함수를 setTimeout 함수를 이용하여 비동기 함수로 만들어보자. 1초 후 'A 작업 끝'을 출력하도록 만들었다.

```
function taskA(){
  setTimeout(() => console.log('A 작업 끝'), 1000);
}

taskA();
console.log('코드 끝');
```

위 코드를 실행해보면 taskA 함수가 실행완료될때까지 기다리지 않고 '코드 끝'이 먼저 출력된 후 taskA 함수의 'A 작업 끝' 문자열이 출력된다.

---

### 자바스크립트의 비동기 방식 동작원리

자바스크립트 코드는 브라우저에 탑재되어 있는 자바스크립트 엔진을 이용해서 해석되고 실행된다.

엔진의 구성요소

* Heap: 변수나 상수들에 사용되는 메모리를 저장하는 영역
* Call Stack: 코드의 실행에 따라 호출 스택을 쌓는 영역

#### 콜스택

자바스크립트 콜스택은 **LIFO(Last-In First-Out)** 방식으로 실행된다.

코드를 직접 수행하는 스레드는 콜스택 하나를 담당하고 콜스택의 작동방식에 따라 명령을 처리하는데 자바스크립트는 콜스택이 하나이므로 **싱글 스레드 방식**이라고 한다.

#### 콜백 큐, Web APIs, 이벤트 루프

자바스크립트 엔진과 웹 브라우저 간 비동기 처리와 같은 상호작용을 처리하기 위한 구성요소.
콜스택에 추가된 작업 중 setTimeout 함수와 같은 작업은 Web APIs에 추가된다. 
Web APIs는 일종의 extra queue라고 생각할 수 있는데, 여기서 delay 시간이 끝나면 그때 setTimeout의 콜백함수가 콜백큐로 푸시가 되고 이벤트 루프에 의해 콜스택에 추가가 된다. 
이때 이벤트 루프는 콜스택에 메인컨텍스트 이외의 다른 함수가 남아있는지 확인 후 더이상 실행할 함수가 없을때 콜백큐에 있는 콜백함수를 콜스택으로 푸시한다.

![](https://velog.velcdn.com/images/tmdgml110806/post/639dc878-d3bb-437b-85f9-5f02e33d8aeb/image.png)

--- 

### 콜백지옥

![](https://velog.velcdn.com/images/tmdgml110806/post/6242f2e0-88e2-45c8-9fe2-a2e11f252341/image.png)


#### 콜백지옥이란

콜백으로 리턴 받은 값을 또 새로운 콜백함수에 넘겨주고 그 함수의 리턴값을 또 새로운 콜백함수에 넘겨주고 하는 식의 연속적으로 비동기 함수들을 처리할때 **비동기 처리의 결과값을 사용하기 위해서 콜백이 계속 깊어지는 현상**을 말한다.
```
// callback hell 예시
function addNumber(num1, num2, cb){
	setTimeout(() => {
      const res = num1 + num2;
      cb(res);
    }, 2000);
}

function timesTwo(num, cb){
  setTimeout(() => {
      const res = num * 2;
      cb(res);
    }, 3000);
}

function makeNegative(num, cb){
  setTimeout(() => {
      const res = num * -1;
      cb(res);
    }, 4000);
}

addNumber(3, 4, (addedNumberRes) => { // addNumber 함수 실행
  console.log('addNumber result: ', addedNumberRes);
  // timesTwo(콜백함수) 실행 (addNumber 함수의 리턴값(addedNumberRes)을 인자로 넘겨줌)
  timesTwo(addedNumberRes, (timesTwoRes) => {
    console.log('timesTwo result: ', timesTwoRes);
    // makeNegative(콜백함수) 실행 (timesTwo 함수의 리턴값(timesTwoRes)을 인자로 넘겨줌)
    makeNegative(timesTwoRes, (makeNegativeRes) => {
      console.log('makeNegative result: ', makeNegativeRes);
       // 더 많은 콜백함수가 추가될 수 있다..
    })
  }
}

```
---
#### Promise를 이용한 콜백지옥 탈출

ES6에서 추가된 Promise 객체를 활용해서 콜백지옥을 해결할 수 있다.

**Promise 객체가 가질 수 있는 3가지 상태**

* Pending (대기상태)
* Fulfilled (성공) - 비동기 작업이 성공적으로 완료된 경우
* Rejected(실패) - 서버 미응답 혹은 응답시간이 초과하여 자동 취소되는 경우 등이 해당됨

**Promise 만들어보기**

Promise가 어떻게 동작하는지 알아보기 위해 비동기 함수인 setTimeout 함수를 활용해서 인자로 값을 받았을때 그 값이 숫자인지 아닌지를 알려주는 함수를 예로 들어보자.

비동기 처리 결과를 성공(resolve), 실패(reject)로 핸들링하기

```
function isNumber(param, resolve, reject){
      setTimeout(() => {
      if(typeof param === 'number'){
        // resolve - 작업이 성공한 경우 실행하는 콜백함수
        resolve('입력하신 값은 숫자입니다.');
      } else {
        // reject - 작업이 실패한 경우 실행하는 콜백함수
        reject('입력하신 값은 숫자가 아닙니다.');
      }
    }, 2000);
 }

isNumber(10,
         (res) => {
  			console.log(`작업 성공: ${res}`)
		}, // resolve 됐을때 실행할 함수
         (error) => {
  			console.log(`작업 실패: ${error}`)
		} // reject 됐을때 실행할 함수
        ) 
		// 2초뒤, 10이 출력된다.
```

**Promise 객체를 사용하여 비동기처리 핸들링하기**

```
function isNumberP(param){
  // 비동기 작업을 실질적으로 수행하는 함수  
  const executor = (resolve, reject) => {
      setTimeout(() => {
      if(typeof param === 'number'){
        // resolve - 작업이 성공한 경우 실행하는 콜백함수
        resolve('입력하신 값은 숫자입니다.');
      } else {
        // reject - 작업이 실패한 경우 실행하는 콜백함수
        reject('입력하신 값은 숫자가 아닙니다.');
      }
    }, 2000);
  }
  
  // Promise 객체를 생성하고 생성자로 비동기 실행 함수를 넘겨준다.
  const asyncTask = new Promise(executor);
  // Promise 객체 return
  return asyncTask;
}

const res = isNumberP(5); // return 받은 Promise 객체를 res에 담는다
res.then((res) => console.log(`작업 성공: ${res}`)
   .catch((error) => console.log(`작업 실패: ${error}`); // 작업 성공: 입력하신 값은 숫자입니다.
          
// Promise의 결과가 reject일 경우 catch문의 콜백이 실행된다.
```

**Promise로 콜백지옥 벗어나기**
"4-1. 콜백지옥이란" 예시 코드의 콜백지옥을 Promise를 사용해서 해결해보자.
```
function addNumber(num1, num2){
  // Promise 객체 생성자에 executor(비동기 실행함수) 함수를 넣어준다
  return new Promise((resolve, reject) => {
    setTimeout(() => {
        const res = num1 + num2;
        resolve(res);
      }, 2000))
  }
}

function timesTwo(num){
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        const res = num * 2;
        resolve(res);
      }, 3000);
    })  
}

function makeNegative(num){
  return new Promise((resolve, reject) => {
  	setTimeout(() => {
      const res = num * -1;
      resolve(res);
    }, 4000);
  })
}
```
프로미스 객체를 반환하는 비동기 함수로 만들었으므로 then 메소드를 사용해서 리턴값을 사용할 수 있다.
```
addNumber(3, 4).then((addedNumberRes) => {
	console.log('addNumber result: ', addedNumberRes);
  	timesTwo(addedNumberRes).then((timesTwoRes) => {
      console.log('timesTwo result: ', timesTwoRes);
      makeNegative(timesTwoRes).then((makeNegativeRes) => {
        console.log('makeNegative result: ', makeNegativeRes);
      })
    })
})
```
하지만 위처럼 사용하면 콜백지옥과 다를바가 없다. then 메소드 안에 계속해서 콜백을 호출하는 것이 아닌, 프로미스를 반환하는 함수 자체를 리턴해줌으로써 이를 해결할 수 있다.
```
addNumber(3, 4).then((addedNumberRes) => {
    console.log('addNumber result: ', addedNumberRes);
    return timesTwo(addedNumberRes);
  }).then((timesTwoRes) => {
      console.log('timesTwo result: ', timesTwoRes);
    return makeNegative(timesTwoRes);
  }).then((makeNegativeRes) => {
      console.log('makeNegative result: ', makeNegativeRes);
  })
})
```

---

### async, await

함수 앞에 async를 붙이면 해당 함수는 프로미스를 반환하게 된다.
```
async function sayHello(){
  return 'Hello world';
}

sayHello().then((res) => console.log(res)); // Hello world
```
delay 함수를 만들어서 3초 뒤에 'Hello world'를 출력하도록 해보자.
```
function delay(ms){
 return new Promise((resolve) => {
   setTimeout(resolve, ms)
 }); 
}

async function sayHello(){
  return delay(3000).then(() => {
    return 'Hello world';
  })
}

sayHello().then((res) => console.log(res)); // 3초 뒤 'Hello world' 출력
```
sayHello 함수를 async, await 키워드를 사용하여 좀더 간결하게 작성할 수 있다.
```
async function sayHello(){
  await delay(3000);
  return 'Hello world';
}
```
await 사용하면 마치 동기적으로 코드가 실행되는 것처럼 동작한다. 위의 코드에서 delay 함수의 작업이 완료되고 나서 리턴문이 실행된다.
```
function delay(ms){
 return new Promise((resolve) => {
   setTimeout(resolve, ms)
 }); 
}

async function sayHello(){
  await delay(3000);
  return 'Hello world'; // 3초 후 'Hello world' 리턴
}

function main(){
	const res = await sayHello(); 
  	console.log(res); // sayHello 함수가 완료된 후 콘솔 출력
}

main();
```
### API 호출하기

API를 호출한다는 것은 서버에 데이터를 요청하는 행위이다. 요청한 데이터는 서버의 상태에 따라 응답 받는 시간이 다르다. 따라서 프로미스를 사용해 비동기적으로 처리한다.
```
async function logJSONData() {
  const response = await fetch("http://example.com/movies.json"); // fetch API는 프로미스를 반환하다.
  const jsonData = await response.json();
  console.log(jsonData);
}

logJSONData();
```
