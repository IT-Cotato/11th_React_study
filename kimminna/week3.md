# Axios란?

리액트는 효율적인 UI 구현을 위한 라이브러리다. HTTP Client(HTTP 상에서 커뮤니케이션하는 자바 기반 컴포넌트)를 내장하고 있는 Angular와는 다르게, 리액트에는 따로 내장 클래스가 없다. 따라서 리액트에서 AJAX(비동기 웹 어플리케이션)을 구현하기 위해서 자바스크립트 내장 객체인 XMLRequest를 사용하거나, 다른 HTTP Client를 사용해야 한다. 

Axios는 node.js와 브라우저를 위한 Promise 기반 HTTP 클라이언트다. 서버 사이드에서는 네이티브 node.js의 http 모듈을 사용하고, 클라이언트(브라우저)에서는 XMLHttpRequests를 사용한다.

쉽게 말해서 백엔드랑 프론트엔드랑 통신을 쉽게 하기 위해 Ajax와 더불어 사용한다. 

## AJAX

AJAX란 자바스크립트의 라이브러리 중 하나로 Asynchronous(비동기) JavaScript AndXML(비동기 자바스크립트와 xml)의 약자다. 브라우저가 가진 XMLHttpRequest 객체를 이용하여 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드하는 기법으로, 자바스크립트를 사용한 통신, 클라이언트와 서버 간의 XML 데이터를 주고받는 기술이다. 정리하자면, 자바스크립트를 통해서 서버에 데이터를 요청하게 한다. 

- **비동기 방식이란?**
    
    비동기 방식은 웹 페이지에 리로드를 하지 않아도 데이터를 불러와주는 방식이며, Ajax를 통해 서버에 요청을 한 후 멈추는 것이 아닌 그 프로그램을 계속 돌린다는 의미를 가진다. 

동기 방식은 결과가 주어질 때까지 아무것도 못하고 대기해야 하는 단점이 있지만, 비동기 방식은 결과가 주어지는 데 걸리는 시간 동안 다른 작업을 병행할 수 있다는 장점과 자원을 효율적으로 사용할 수 있다는 장점을 가진다. 

## Axios vs Fetch API

일반적으로 자바스크립트에서 API를 연동하기 위해서는 보통 Fetch API를 사용하곤 했다. 리액트도 자바스크립트 빌트인 라이브러리 중 하나인 Fetch API라는 모듈을 사용한다. 

하지만 Fetch API가 자바스크립트 빌트인 라이브러리라는 특성 때문에 Axios를 사용하는 것을 선호한다. 

## Axios 특징

- 브라우저를 위해 XMLHttpRequests 객체를 생성
- node.js를 위해 http API 사용
- Promise API를 지원
- 요청 및 응답 인터셉트
- 요청 및 응답 데이터 변환
- 요청 취소
- JSON 데이터 자동 변환(요청 및 응답을 변경함)
- XSRF를 막기 위한 클라이언트 사이드 지원

## 설치 및 사용법

`$ npm install axios (npm 사용 방법)`

생성한 프로젝트 상단에 import로 추가한다.

`import axios from "axios”;`

### HTTP Methods

HTTP Methods는 클라이언트가 웹 서버에게 사용자 요청의 목적/종류를 알려주는 수단이다.

이 메소드들 중에 Axios로 통신하면서 가장 많이 사용되는 메소드는 다음과 같다.

- GET - `axios.get(url,[,config])`
    
    입력한 url이 존재하는 자원에 요청을 보낸다.
    
    서버에서 어떤 데이터를 가져와서 보여줄 것인가를 정하는 용도로 쓴다.
    
    주소에 있는 쿼리스트링을 활용해 정보를 전달하고, GET 메서드에서는 값이나 상태 등을 직접 바꿀 수는 없다.
    
    ```jsx
    import axios from "axios"
    
    axios.get('https://localhost:3000/login/user')
      .then((Response)=>{console.log(Response.data)})
      .catch((Error)=>{console.log(Error)})
    ```
    
    응답은 JSON 형태로 넘어온다. 
    
- POST - `axios.post(”url주소”,{data객체},[,config])`
    
    새로운 리소스를 생성할 때 사용한다.
    
    두 번째 인자에서는 본문으로 보낼 데이터를 설정한 객체 리터럴을 전달한다. 
    
    Post를 사용하면 주소 창에 쿼리스트링이 남지 않고 GET보다 안전하다. 
    
    ex. 업로드, 로그인, 글 작성 
    
    ```jsx
    axios.post( 'url', 
     {
       contact: 'JungHo',
       email: 'wjdgh0727@gmail.com'
     },
     {
       headers: {
          'Content-type': 'application/json',
          'Accept': 'application/json'
       }
      }
     ) 
       .then((response) => {console.log(response.data); })
       .catch((response) => {console.log('Error!') });
    ```
    
- DELETE - `axios.delete(url,[,config]);`
    
    REST 기반 API 프로그램에서 데이터베이스에 저장된 내용을 삭제하는 목적으로 사용된다. Delete 메소드는 서버에 있는 데이터베이스의 내용을 삭제하는 것을 주 목적으로 하기 때문에 두 번째 인자를 아예 전달하지 않는다.
    
    ```jsx
    axios.delete('/thisisExample/list/30").then(function(response){
      console.log('삭제성공',response.data);
       }).catch(function(ex) {
        throw new Error(ex)
     }
    ```
    
- PUT - `axios.put(url[, data[, config]])`
    
    REST 기반 API 프로그램에서 데이터베이스에 저장된 내용을 갱신하는 목적으로 사용된다.  HTML Form 태그에 기본적으로 지원하는 HTTP 메서드는 아니다.
    

## Axios 인스턴스 만들기

API를 여기저기서 일정한 형식 없이 불러오면 재사용성이 떨어지기 때문에 코드의 가독성을 위해 Axios 모듈화를 진행하는 것이 좋다. 


### 인스턴스 생성

.create(config) 메소드를 사용하여 사용자 정의 구성을 사용하는 axios 인스턴스를 생성할 수 있다.

```jsx
const instance = axios.create({
  baseURL: "https://api.themoviedb.org/3", // 계속 반복되는 부분
  params: {
    api_key: "31df71945f1eda5c2e2a86bfcab7070c",
    Language: "ko-KR",
  },
});
```

## 사용 예시

```jsx
const [movie, setMovie] = useState([]);

const fetchData = async () => {
    // 현재 상영 중인 영화 정보 가져오기(여러 영화들)
    const request = await axios.get(requests.fetchNowPlaying);
    // 여러 영화 중 영화 하나의 ID를 가져오기
    const movieId =
      request.data.results[
        Math.floor(Math.random() * request.data.results.length)
      ].id;

    // 특정 영화의 더 상세한 정보를 가져오기(비디오 정보 포함)
    const { data: movieDetail } = await axios.get(`movie/${movieId}`, {
      params: { append_to_response: "videos" },
      // 받아오는 response에 비디오도 넣어서 전달해달라는 것
    });

    setMovie(movieDetail);
  };

```

## async와 await

async와 await라는 특별한 문법을 사용하면 프라미스를 좀 더 편하게 사용할 수 있다. 

### async 함수

```jsx
async function f() {
	return 1;
}
```

함수 앞에 async를 붙이면 해당 함수는 항상 프로미스를 반환한다. 

### await 키워드

```jsx
let value = await promise;
```

자바스크립트는 await 키워드를 만나면 프로미스가 처리될 때까지 기다린다. 

```jsx
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000)
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  alert(result); // "완료!"
}

f();
```

함수를 호출하고, 함수 본문이 실행되는 도중에서 await 키워드로 작성된 코드 실행이 잠시 중단되었다가 프로미스가 처리되며 실행이 재개된다. 이때 프로미스 객체의 result 값이 변수 result에 할당된다. 따라서 위 예시를 실행하면 1초 뒤에 완료! 가 출력된다. 

await은 말 그대로 프로미스가 처리될 때까지 함수 실행을 기다리게 만든다. 프로미스가 처리되면 그 결과와 함께 실행이 재개된다. 프로미스가 처리되길 기다리는 동안에 엔진이 다른 일(다른 스크립트를 실행, 이벤트 처리) 등을 할 수 있기 때문에 CPU 리소스가 낭비되지 않는다. 

await은 promise.then보다 조금 더 세련되게 프로미스의 result값을 얻을 수 있도록 해 주는 문법이다. promise.then보다 가독성도 좋고 쓰기도 쉽다. 

일반 함수에는 await을 사용할 수 없다는 점에 유의한다. 비동기 함수 안에서만 쓴다. (async 함수 안에서만)

# Styled Components

### CSS in JS

CSS in JS는 스타일 정의를 CSS 파일이 아닌 자바스크립트로 작성된 컴포넌트에 바로 삽입하는 스타일 기법이다. 

기존에 웹사이트를 개발할 때는 html, css, js를 각자 별도의 파일에 두는 것이 좋다고 여겼지만 리액트나 vue, angular와 같은 모던 자바스크립트 라이브러리가 인기를 끌면서 최근에는 웹 애플리케이션을 여러 개의 재활용이 가능한 빌딩 블록으로 분리하여 개발하는 컴포넌트 기반 개발 방법이 주류가 되고 있다.

따라서 웹페이지를 html, css, js 세 개로 분리하는 것이 아니라 여러 개의 컴포넌트로 분리하고, 각 컴포넌트에 html, css, js를 전부 삽입하는 패턴이 많이 사용되고 있다. 

# 설치 및 사용법

`$ npm i styled-components`

설치 후 package.json 파일에 styled-components가 추가된 것을 확인할 수 있다.

```jsx
import styled from "styled-components";

styled.button`
  // <button> HTML 엘리먼트에 대한 스타일 정의
`;

styled(Button)`
	// <Button /> React component에 대한 스타일 정의
`;
```

styled-components 패키지에서 styled 함수를 import 한다. 

styled는 HTML 엘리먼트나 React 컴포넌트에 원하는 스타일을 적용하기 위해서 사용된다. 

기본 문법은 HTML, React 중 어떤 것을 스타일링하느냐에 따라 조금씩 다르다. 

## props를 이용해서 가변 스타일링하기

styled components는 리액트 컴포넌트에서 넘어온 props에 따라 다른 스타일을 적용하는 기능을 제공한다. 

styled component는 tagged template literal을 사용하기 때문에 함수도 문자열 안에 포함시킬 수 있다는 점을 이용해서 작성해 줄 수 있다. 

- tagged teplate literal
    
    템플릿 리터럴 앞에 함수를 붙여서 호출하는 방식이다.
    
    JS가 문자열과 값들을 쪼개서 함수에 전달해 준다. 
    

```jsx
import React from "react";
import styled from "styled-components";

const StyledButton = styled.button`
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;

  color: ${(props) => props.color || "gray"};
  background: ${(props) => props.background || "white"};
`;

function Button({ children, color, background }) {
  return (
    <StyledButton color={color} background={background} Î>
      {children}
    </StyledButton>
  );
}
```

컴포넌트에서 받아온 prop의 색상에 따라 다른 CSS 스타일링을 적용하는 예시이다. 

prop이 넘어오지 않은 경우를 대비해, 자바스크립트의 || 연산자를 이용하여 기존에 정의한 기본 색상이 유지되도록 한다.