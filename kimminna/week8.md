# Next.js

https://nextjs.org/

풀스택 웹 애플리케이션을 구축하기 위한 리액트 기반 프레임워크.

프론트엔드의 UI는 리액트로 구성하며, Next.js는 페이지 라우팅, 서버 사이드 렌더링, 정적 사이트 생성 및 성능 최적화와 같은 추가 기능을 제공한다. 또한 Next.js는 번들링, 컴파일 등의 작업을 추상화하고 자동으로 처리해, 개발자가 설정에 시간을 들이지 않고 애플리케이션 개발에 집중할 수 있도록 돕는다.

SPA를 이용하며 Client Side Rendering을 하는 데에 좋은 점도 있지만 단점도 있는데, 그 부분이 바로 검색 엔진 최적화 부분이다.

Client Side Rendering을 하면 첫 페이지에서 빈 html을 가져와서 js 파일을 해석하여 화면을 구성하기 때문에 포털 검색에 거의 노출될 일이 없다. 하지만 Next.js에서는 Pre-rendering을 통해서 페이지를 미리 렌더링하여 완성된 HTML을 가져오기 때문에 사용자와 검색 엔진 크롤러에게 바로 렌더링된 페이지를 전달할 수 있게 된다.

리액트에서도 SSR을 지원하지만 구현하기에 굉장히 복잡하기 때문에 Next.js를 통해서 이 문제를 해결해 주게 된다.

- 설치: `npx create-next-app@latest`

# Next.js에서 데이터를 가져오는 방법

보통 리액트에서는 데이터를 가져올 때 useEffect 안에서 가져오지만, Next.js에서는 다른 방법을 사용해서 가져오곤 한다.

1. **getStaticProps - Static Generation으로 빌드할 때 데이터를 불러온다.**

   ```tsx
   export async function getStaticProps(context) {
     return {
       props: {},
     };
   }
   ```

   getStaticProps 함수에서 리턴되는 props를 가지고 페이지를 pre-render 한다.

   useEffect로 데이터를 가져올 때보다 훨씬 빠르다.

   페이지가 미리 렌더링되어야 하고 매우 빨라야 할 때 사용하면 좋다.

2. **getStaticPaths - Static Generation으로 데이터에 기반하여 pre-render 시 특정한 동적 라우팅을 구현한다. (pages/post/[id].js)**

   ```tsx
   export async function getStaticPaths(){
   	return {
   		paths: [
   			{ params: {...} }
   		],
   		fallback: true
   	};
   }
   ```

   동적 라우팅이 필요할 때 getStaticPaths로 경로 리스트를 정의하고, HTML에 빌드 시간에 렌더된다.

   - paths
     - 어떠한 경로가 pre-render 될지 결정
   - params
     - 페이지 이름이 pages/posts/[postId]/[commentId] 라면 params는 postId와 commentId
     - 페이지 이름이 pages[…slug] 와 같이 모든 경로를 사용한다면 params는 slug가 담긴 배열 - [’postId’, ‘commentId’]
   - fallback
     - 값이 false - getStaticPaths로 리턴되지 않는 것은 모두 404 페이지가 뜸
     - 값이 true - getStaticPaths로 리턴지 않은 것은 404가 아닌 fallback 페이지가 뜸
       fallback 시 어떤 페이지를 보여줄 지 개발자가 미리 정의 가능

3. **getServerSideProps - Server Side Rendering으로 요청이 있을 때 계속 데이터를 불러온다**.

   ```tsx
   export async function getServerSideProps() {
     const res = await fetch(`https://.../data`);
     const data = await res.json();
     return {
       props: { data },
     };
   }
   ```

   각 요청마다 리턴되는 데이터를 getServerSideProps로 pre-render한다.

   요청할 때 데이터를 가져와야 하는 페이지를 미리 렌더링할 때 사용한다.

## Static Site Generation(SSG)

npm run build 처럼 빌드 타임 때 HTML을 각 페이지 별로 서버에 생성해 놓고 요청이 왔을 때 생성된 HTML을 반환한다.

그래서 브라우저가 페이지를 요청했을 때 이미 생성된 HTML만 반환하고 재사용할 수도 있게 된다. 따라서 응답 속도가 매우 빠르다.

외부 요청에 의해서 변동이 없는 페이지들은 먼저 만들어 놓고 그것을 재사용하면 좋다.

# TypeScript 개론

# TypeScript란?

마이크로소프트에서 개발한 오픈 소스 프로그래밍 언어로, 자바스크립트의 상위 집합이다. 타입스크립트는 자바스크립트 코드에 정적 타입을 추가하여 코드의 안전성과 유지보수성을 높이는 것을 목표로 한다. 타입 시스템을 통해서 개발자가 컴파일 단계에서 많은 오류를 발견할 수 있으며, 특히 대규모 애플리케이션 개발에서 매우 유용하다.

## JavaScript vs TypeScript

자바스크립트는 동적 타입 언어인 반면, 타입스크립트는 정적 타입 언어다.

자바스크립트는 변수의 타입이 런타임에 결정된다.

이는 유연성은 확실히 제공하지만, 잘못된 타입 사용으로 인해 예기치 않은 오류가 발생할 수 있다는 단점이 있다.

반면, 타입스크립트는 변수의 타입이 컴파일 타임에 결정된다. 개발자는 변수, 함수의 인자, 반환값 등에 타입을 명시적을 지정할 수 있으며, 컴파일러는 이 정보를 바탕으로 타입 검사를 수행한다. 이를 통해 잘못된 타입 사용으로 인한 오류를 컴파일 단계에서 미리 발견할 수 있다.

자바스크립트는 인터브리터 언어로, 코드를 한 줄씩 해석하고 실행한다. 빠른 실행이 가능하지만, 코드에 오류가 있을 경우 런타임에서만 발견할 수 있다.

반면, 타입스크립트는 컴파일러를 사용해서 코드를 자바스크립트로 변환한 후 실행된다. 컴파일러는 코드 전체를 분석하여 오류를 미리 감지할 수 있기 때문에 더 안전한 코드 작성이 가능하다.

# 타입

자바스크립트에서 기본적으로 제공하는 기본 제공 유형들을 상속한다.

타입스크립트 유형은 다음과 같이 분류된다 .

![image.png](attachment:44be78b2-ee5a-4a1c-9384-2a336b943fed:image.png)

### Primitive types

![image.png](attachment:5247c38d-99b0-49ce-a8fe-3aaaf7903d51:image.png)

### Object types

![image.png](attachment:d5abf26c-8ec8-4e3f-8209-ce276695c599:image.png)

### 추가 타입들

- **Any**
  잘 알지 못하는 타입을 표현해야 할 때 사용한다. (사용자로부터 받은 데이터나, 서드파티 라이브러리 같은 동적인 컨텐츠) 하지만 Any는 최대한 쓰지 않는 게 좋다.
  ```tsx
  let something: any = "hello world!";
  something = 23;
  something = true;
  ```
- **Union**
  변수 또는 함수 매개변수에 대해 둘 이상의 데이터 유형을 사용할 수 있다.
  ```tsx
  let code: string | number;
  code = 123; // ok
  code = "123"; // ok
  code = false; // compile error
  ```
- **Tuple**
  배열 타입을 보다 특수한 형태로 사용할 수 있다. tuple에 명시적으로 지정된 형식에 따라 아이템 순서를 설정해야 되고, 추가되는 아이템들도 tuple에 명시된 타입들만 가능하다.
  ```tsx
  var employee: [number, string] = [1, "Steve"];

  employee.push(2, "bill");
  console.log(employee); // [1, 'Steve', 2, 'bill]

  employee.push(true); // compile error
  ```
- **Enum**
  값들의 집합을 명시하고 이를 사용하도록 한다.
  js의 object와는 달리 선언 이후에 변경할 수 없고, enum은 속성 값을 문자열 혹은 숫자만 허용된다.
  ```tsx
  enum PrintMedia {
    Newspaper, // 할당 안 된 경우 자동으로 0, 1, 2, 3
    Newsletter, // 할당도 가능
    Magazine,
    Book,
  }

  let mediaType: number = PrintMedia.Book; // 3
  ```
- **Void**
  데이터가 없는 경우 void를 사용한다. 예를 들어 함수가 값을 반환하지 않는 경우 반환 타입으로 void를 지정할 수 있다.
  타입이 없는 상태이며, any와는 반대의 의미를 가진다.
  ```tsx
  function sayHi(): void {
    console.log("hi");
  }

  let speech: void = sayHi();
  console.log(speech); // undefined
  ```
- **Never**
  절대 발생하지 않을 값을 나타낸다.
  일반적으로 함수의 리턴 타입으로 사용되는데, 항상 오류를 리턴하거나 리턴 값을 절대로 내보내지 않음을 의미한다. (이는 무한 루프에 빠지는 것과 같다.)
  ```tsx
  function throwError(errorMsg: string): never {
    throw new Error(errorMsg);
  }
  ```
- … 등등

## Type annotation / Type inference / Type assertion

- **Type annotation**
  개발자가 타입을 직접 명시
  ```tsx
  const rate: number = 5;
  ```
- **Type inference**
  타입스크립트가 알아서 타입을 추론
  ```tsx
  const rate = 5;
  ```
  - 타입 추론이 불가하기 때문에 타입 명시가 필요한 경우들
    - any 타입을 리턴하는 경우
    - 변수 선언을 먼저 하고 나중에 초기화하는 경우
    - 변수에 대입될 값이 일정하지 않은 경우
- **Type assertion**
  시스템이 추론 및 분석한 타입 내용을 개발자가 마음대로 바꿈
  값의 타입을 설정하고 컴파일러에게 이를 유추하지 않도록 지시할 수 있다.
  ```tsx
  interface Foo {
    bar: number;
    bas: string;
  }

  var foo = {} as Foo;
  foo.bar = 123;
  foo.bas = "hello";
  ```
  as Foo 혹은 <Foo> 로 타입 표명을 표현할 수 있지만 리액트를 사용할 때에는 <> 표시가 문법의 혼란을 야기하기 때문에 as Foo 로 사용하도록 한다.
