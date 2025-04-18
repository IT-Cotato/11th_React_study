# 페이지 라우팅

# 라우팅

라우팅이란 네트워크에서 경로를 선택하는 프로세스다. 컴퓨터 네트워크는 노드라는 여러 시스템과 이러한 노드를 연결하는 링크로 구성된다. 상호 연결된 네트워크에서 노드 간의 통신은 여러 경로를 통해 이루어질 수 있는데, 라우팅은 이를 정해진 규칙에서 최상의 경로를 찾는 프로세스다. 

# 클라이언트 사이드 라우팅

리액트 라우터는 클라이언트 사이드 라우팅을 가능케 한다. 

기존의 웹 사이트들은 서버 사이드 라우팅을 사용했다. 이는 서버에 HTTP를 요청하여 새로운 엔드포인트를 설정하고, CSS와 JS를 다운로드하고, 받은 HTML을 렌더링하는 방식으로 이루어진다. 그리고 사용자가 링크를 클릭할 시 그에 따른 새로운 프로세스를 다시 시작한다. 

클라이언트 사이드 라우팅을 사용할 경우 서버에 새로운 요청 없이 링크를 클릭하여 가상의 엔드포인트로 URL을 업데이트할 수 있다. 그리고 어플리케이션은 새로운 UI를 렌더링하고, 데이터 요청을 통해 페이지를 새롭게 업데이트한다. 

기존의 서버 사이드 라우팅처럼 새로운 CSS와 JS를 다운받고 다시 해석하는 과정을 거치지 않기 때문에 훨씬 빠른 사용자 경험을 가능하게 한다. 실제로 서버 사이드 라우팅 방법으로 만들어진 웹사이트들은 페이지가 이동할 때 깜빡거리면서 이동한다. 클라이언트 사이드 라우팅을 이용하면 부드럽게 넘어가진다. 

이는 한 어플리케이션에서 하나의 HTML만 받아서 실행시키므로 페이지 간 분기점이 생기지만 Single Page Application의 근본적인 동작 원리에는 영향을 끼치지 않는다. 

또한, SPA를 사용하면서도 URL을 통한 엔드포인트 설정이 가능해져서 어플리케이션의 특정 페이지를 URL을 통해 바로 접근할 수 있다는 이점도 생긴다. 

# 라우팅 사용하기

1. **프로젝트에 리액트 라우터를 설치한다.**
    
    터미널을 통해 프로젝트 디렉토리 루트로 이동하여 아래 명령어를 이용하여 설치한다.
    
    `npm i react-router-dom`
    
2. **프로젝트에 리액트 라우터를 적용한다.**
    
    어플리케이션의 최상위 컴포넌트를 리액트 라우터의 내부 컴포넌트인 `BrowserRouter`로 감싸 준다. 리액트 라우터 돔이라는 라이브러리에서 import 해 준 뒤, main.jsx 파일에서 App 컴포넌트를 렌더하는 코드에서 감싸 주면 된다. 
    
    ```jsx
    // main.jsx
    import { createRoot } from "react-dom/client";
    import "./index.css";
    import App from "./App.jsx";
    import React from "react";
    import { BrowserRouter } from "react-router-dom";
    
    createRoot(document.getElementById("root")).render(
      <BrowserRouter>
        <App />
      </BrowserRouter>
    );
    ```
    
3. **컴포넌트에 Route로 라우팅 시킨다.**
    
    어플리케이션을 감싼 이후에는 리액트 라우터의 또다른 내부 컴포넌트인 `Routes`와  `Route`를 통하여 경로에 따라 불러올 컴포넌트를 지정할 수 있다. 사용을 위해 react-router-dom 라이브러리에서 Route와 Routes 컴포넌트를 import 한다.
    
    Routes 컴포넌트 안에는 Route 컴포넌트만 들어갈 수 있다. Routes 컴포넌트 밖에 위치한 요소들은 페이지 경로에 상관없이 모든 페이지에 렌더링된다. (헤더, 푸터나 고정적인 요소들에 적용하면 좋다.)
    
    Route 컴포넌트에 특별한 prop인 path를 지정한다. 이는 문자열을 값으로 가지며, 부여된 값은 Route의 내부 컴포넌트들을 렌더링할 조건인 url 경로를 의미한다. path 속성에 *를 넣을 경우 어떤 경로에도 해당되지 않는 페이지를 렌더링하면 된다. (404 not found 같은)
    
    또한, 이러한 방식으로 렌더링되는 페이지 컴포넌트 같은 경우에는 프로젝트의 디렉토리에서 /pages 같은 폴더를 생성하여 일반 컴포넌트들과 분리시키는 것이 권장된다. 
    
    ```jsx
    
    import { Route, Routes } from "react-router-dom";
    function App =() => {
    // ...
      return (
        <>
          <Routes>
            // Routes 컴포넌트 안에는 Route 컴포넌트만 들어갈 수 있다. // Routes
            컴포넌트 밖에 위치한 요소는 페이지 경로에 상관없이 모든 페이지에
            렌더링된다. // 경로가 동적 경로인 url parameter를 사용할 것임을 명시 -
            경로 뒤에 :id 와 같이 표시한다.
            <Route path="/" element={<Home />} />
            <Route path="/new" element={<New />} />
            <Route path="/diary/:id" element={<Diary />} />
            <Route path="/edit/:id" element={<Edit />} />
            <Route path="*" element={<Notfound />} /> // *: 와일드카드. 위의 경로와
            일치하지 않을 경우
          </Routes>
        </>
      );
    }
    
    export default App;
    
    ```
    
4. **링크를 통해 페이지 이동시키기** 
    
    기존의 HTML에서 링크를 통해 페이지를 이동시킬 때는 `<a href=’url’>` 이용하여 이용했다. 이는 리액트 라우터에서도 유효하긴 하지만, a 태그는 동일 HTML 내에서 렌더링되는 것이 아니라 새로운 HTML을 다운받아 렌더링된다. 이러한 현상을 방지하기 위해 이제부터는 리액트 라우터 내부 컴포넌트인 `<Link>`를 사용한다. 기존에 a 태그를 사용하던 부분에 대치하여 적용시킨 후, `href=’url’` 어트리뷰트를 `to=’url’`로 바꿔 작성한다.
    
    `<Link>` 컴포넌트를 사용할 경우 HTML 상에서는 동일한 a 태그로 렌더링되지만 서버 통신을 통해 페이지가 아예 새로 불러지는 것을 방지한다. 
    
    ```jsx
    import { Route, Routes, Link } from "react-router-dom";
    function App =() => {
    // ...
      return (
        <>
    	    <div>
    		    <Link to = "/welcome"> Welcome </Link>
    		   <div>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/new" element={<New />} />
            <Route path="/diary/:id" element={<Diary />} />
            <Route path="/edit/:id" element={<Edit />} />
            <Route path="*" element={<Notfound />} /> // *: 와일드카드. 위의 경로와
            일치하지 않을 경우
          </Routes>
        </>
      );
    }
    
    export default App;
    ```
    

# 동적 경로 라우팅하기

쇼핑몰 웹 사이트를 가정해 본다. 상품이 나열되는 상품 리스트 컴포넌트를 만들 테고, 상품 리스트 내에 존재하는 상품을 클릭 시 상품의 상세 페이지로 이동하도록 구현할 것이다. 

이때 리액트 라우터를 사용한다면 지금까지 알아본 내용과 같이 <Link> 컴포넌트를 통해 라우팅되는 상세 페이지를 렌더링할 것이다. 

웹 사이트의 url을 들어가 보면, 특정 상품에 대한 id 코드가 존재하는 걸 흔히 볼 수 있다. 즉, 페이지가 특정한 id를 받아서 렌더링을 했다고 볼 수 있는 것이다.

이처럼 라우팅 과정에서 페이지에 유동적인 값을 부여하여 라우팅하는 것을 동적 라우팅이라고 한다. 

## 동적 경로의 종류

![image.png](attachment:f525bdb8-a8ed-4012-b76d-23aca3d2a9e1:image.png)

1. **URL 파라미터**
    
    주로 id나 이름을 이용하여 특정 데이터를 조회할 때 사용한다. 
    
    ```
    https://localhost:3000/diary/{id}
    //id가 3일 경우
    https://localhost:3000/diary/3
    ```
    
2. **쿼리 스트링**
    
    물음표 뒤에 key = value 문법으로 URL에 유동적인 값을 포함하는 방법이다. 주로 게시물 리스트에서 사용자가 정렬 조건을 선택하거나, 현재 조회하는 게시판의 페이지를 표현할 때 사용한다. 혹은 검색창에서 사용자의 입력을 받아서 페이지를 표시하는 등 바뀔 여지가 많은 페이지들에 대해 사용한다. (고정적이지 않은 값들에 대해서)
    
    ```
    https://localhost:3000?sort=latest
    
    //값이 2개 이상일 경우
    https://localhost:3000?sort=lates&page=1
    ```
    

## 동적 경로 적용하기

1. **URL 파라미터**
    
    Route 컴포넌트의 path props에 경로를 전달한다. id 자리에 임의의 숫자를 삽입했을 때 그에 맞는 해당 페이지에 렌더링된다. 
    
    그리고 불러올 페이지 컴포넌트에서 `useParams`라는 리액트 커스텀 훅을 라이브러리에서 불러온다. 
    
    useParams는 파라미터 값을 URL을 통해서 넘겨받은 페이지에서 사용할 수 있게끔 한다. 
    
    ```jsx
    import { Route, Routes, Link } from "react-router-dom";
    function App =() => {
    // ...
      return (
        <>
    	    <div>
    		    <Link to = "/welcome"> Welcome </Link>
    		   <div>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/new" element={<New />} />
            <Route path="/diary/:id" element={<Diary />} />
            <Route path="/edit/:id" element={<Edit />} />
            <Route path="*" element={<Notfound />} /> 
          </Routes>
        </>
      );
    }
    
    export default App;
    
    -------------------------------------------------
    // URL 파라미터 값 불러오기 
    
    import { useParams } from "react-router-dom";
    
        const Diary = () => {
          const { id } = useParams();
          return (
            <div>
              <div>{id}번 일기</div>
              <div>Diary 페이지입니다</div>
            </div>
          );
        };
    
        export default Diary;
    ```
    

1. **쿼리 스트링** 
    
    쿼리 스트링은 URL 경로 다음에 ? 로 구분하므로 URL 파라미터처럼 페이지 라우팅을 위한 별도의 설정을 하지 않아도 된다. 
    
    불러올 페이지에서 `useSearchParams`라는 리액트 훅을 라이브러리에서 불러온다. 이 훅은 쿼리 스트링으로 전달된 값을 꺼내오는 역할을 한다. 
    
    `const [params, setParams] = useSearchParams();` 처럼 state와 비슷하게 사용한다. 
    
    params에는 쿼리 스트링으로 전달된 변수와 값이 들어간다.
    
    setParams에는 특정 쿼리 스트링으로 변경할 수 있는 함수가 들어간다.