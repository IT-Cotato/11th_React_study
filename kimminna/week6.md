# useDebounce와 검색 최적화

React에서 UI를 구현하다 보면 사용자의 입력에 따라 실시간으로 결과를 업데이트해야 하는 경우가 많다. 예를 들어, 사용자가 검색어를 입력하면 해당 검색어에 따라 결과를 즉시 보여주는 기능이 그런 경우다.

만약 사용자가 아직 입력을 끝마치지 않았는데도 불구하고 매번 키보드 키가 눌릴 때마다 API 요청을 하면 서버에 과부하가 발생할 수 있고, 응답 시간이 길어져 사용자 경험이 떨어질 수 있다.

이런 문제를 해결하기 위한 방법 중 하나로 debounce 기법이 있다.

debouncing이란 연이어 호출되는 함수들 중 마지막 함수(혹은 제일 처음 함수)만 호출되도록 하는 것이다. 즉, 사용자의 연속적인 요청을 그룹화해서 마지막 요청만 처리하는 것이다.

요즘 서비스들은 검색어를 치자마자 엔터 없이도 결과가 바로바로 나오는 걸 볼 수 있는다. ‘리액트’를 검색창에 입력하는 상황을 가정한다. 엔터 없이도 결과를 즉시 보여주려면 항상 input 이벤트에 대기를 해야 한다. 디바운싱을 사용하지 않으면 한 글자를 입력할 때마다 API 요청이 실행된다. ‘ㄹ’, ‘리’, ‘링’, ‘리액’ 을 순서대로 입력할 때마다 API 요청이 실행된다. 디바운싱을 사용해서 사용자가 모든 입력을 끝마쳤을 때 API 요청을 하도록 구현해 본다.

```jsx
import React, { useEffect, useState } from "react";

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState();

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  return debouncedValue;
}

export default useDebounce;
```

위의 useDebounce 커스텀 훅은 value와 delay를 인자로 받아서 debouncedValue라는 상태값을 반환한다. value 값이 변경될 때마다 useEffect 내부의 setTimeout 함수로 설정된 딜레이 후에 deboucnedValue 상태값을 업데이트한다.

만약 delay가 끝나기 전에 value가 또 바뀌면 clearTimeout으로 이전 타이머를 취소하고, 새로 타이머를 시작한다. 이를 통해 입력이 끝난 후 일정 시간 동안 변화가 없을 때만 값이 반영되게 한다.

- 실행 메커니즘
  상황: ‘123’을 순서대로 빠르게 입력할 때
  1. 1을 입력
     - value가 1이 되면서 useEffect 실행
     - setTimeout이 걸려서 delay 후 1을 debouncedValue로 설정할 준비
     - 아직 delay는 끝나지 않은 상태
  2. delay가 끝나기 전 2 입력
     - value가 2가 되면서 useEffect 다시 실행됨
     - 이때 이전 effect의 리턴 함수(clearTimeout)이 먼저 실행됨 - 즉, 1을 반영하려던 타이머는 취소됨
     - 2를 반영하려는 새 타이머 시작
  3. 3 입력
     - 2를 반영하려는 새 타이머도 취소되면서 3으로 새로운 타이머 시작

```jsx

function SearchPage() {
  const [searchResults, setSearchResults] = useState([]);
  const navigate = useNavigate();
  const useQuery = () => {
    return new URLSearchParams(useLocation().search);
  };

  let query = useQuery();
  const searchTerm = query.get("q");
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  useEffect(() => {
    if (debouncedSearchTerm) {
    // API 요청 등 비동기 작업 수행
      fetchSearchMovie(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);

  ...

  const fetchSearchMovie = async (searchTerm) => {
    try {
      const request = await axios.get(
        `search/multi?include_adult=false&query=${searchTerm}`
      );
      setSearchResults(request.data.results);
      console.log(searchResults);
    } catch (error) {
      console.log("error", error);
    }
  };
```

# 모달 창 외부 클릭 핸들링

웹사이트들의 모달 창을 보면, 반드시 ‘닫기’ 버튼을 눌러야만 닫히는 모달과 모달 창 이외의 영역이 클릭되면 모달이 닫히는 두 가지의 경우가 일반적이다.

모달 창의 바깥 영역을 클릭할 때 창이 닫히게 구현하면 사용자 경험을 향상시킬 수 있다.

# 구현 과정

1. 모달 안 / 모달 밖 클릭 영역 구분
2. 모달 창 밖을 클릭하면 callback 함수를 호출하는 이벤트 등록하기
3. callback 함수 안에서 모달 닫아주기

```jsx
const ref = useRef();
useOnClickOutside(ref, () => {
    setModalOpen(false);
      });

 ...


 <div className="modal" ref={ref}></div>


 ...

function useOnClickOutside(ref, handler) {
  useEffect(() => {
    const listener = (event) => {
      if (!ref.current || ref.current.contains(event.target)) return;
      handler(event);
    };

    document.addEventListener("mousedown", listener);
    return () => {
      document.removeEventListener("mousedown", listener);
    };
  }, [ref, handler]);
}

export default useOnClickOutside;


```

- 모달을 참조하는 Ref를 사용하여 useOnClickOutside에 인자로 ref와 핸들러 함수를 호출하게 한다.
- 함수 내부에서는 useEffect를 사용하여 모달의 상태 변경을 감지한다. ref 객체가 이벤트 타겟을 포함하면 그대로 리턴하고, 그렇지 않은 경우(모달 바깥 영역을 클릭한 경우)에는 handler 함수를 호출한다.
- document.addEventListener() : 문서 전체에 대한 클릭 이벤트를 수신하고, 이벤트 발생 시 실행되는 함수를 전달한다.
- document.removeEventListenr(): 클릭 이벤트 리스너를 제거하는 정리 함수로, 메모리 누수를 방지한다. 추가된 이벤트 리스너를 명시적으로 제거하지 않을 경우 컴포넌트가 소멸되더라고 해당 이벤트 리스너가 계속 남아있게 되고, 이는 메모리 누수를 일으킬 수 있다. 따라서 클린업 함수를 이용해 등록한 이벤트 리스너를 꼭 제거해 준다.
  - 주의: 핸들러 함수가 동일해야 제거된다. 익명 함수 x

# useParams / useNavigate / useLocation / useMatch / useSearchParams

React에서는 페이지 이동을 위해 Link 태그를 이용한다. URL이 변하면 Router 컴포넌트에 정의되어 있는 path 값에 따라 그에 맞는 컴포넌트가 불러와지고, 해당 id에 대한 아이템 정보를 요청한다. 이때 필요한 id 값을 받아오는 데 쓰이는 훅은 다음과 같다.

# 1. useParams

현재 URL 경로에서 동적 파라미터 값을 추출할 때 사용한다.

path parameter 정보를 담고 있는 객체를 리턴한다.

```jsx
<Route path="/user/:id" element={<User />} />

------

const { id } = useParams();
console.log(id); // "123" if URL is /user/123
```

# 2. useNavigate

페이지를 이동시키는 (URL을 변경하는) 함수를 반환한다.

인자에 정수 값을 넣어주면 브라우저의 방문 기록에 남아 있는 경로들을 앞뒤로 탐색할 수 있다.

주로 조건이 필요한 곳에서 호출하여 경로를 이동하게끔 한다.

예를 들어 어떤 포스터를 클릭 시 상세 페이지로 이동하게 할 때, 해당 태그에 onClick으로 navigate를 호출하여 url 경로를 변환하여 구현한다.

```jsx
const navigate = useNavigate();

const goHome = () => {
  navigate('/home');
};

---------

navigate(-1); // 뒤로가기
navigate(-2); // 뒤로 2페이지 가기
navigate(1); // 앞으로 가기
```

## Link 컴포넌트 vs useNavigate

Link 컴포넌트는 클릭 시 바로 이동하는 로직을 구현할 때 사용하고, useNavigate는 페이지 전환 시 추가로 처리해야 할 로직이 있을 경우 사용한다. 예를 들어 로그인 버튼을 클릭했을 때, 회원가입이 되어 있는 사용자는 Main 페이지로, 회원가입이 되어 있지 않은 사용자는 SignUp 페이지로 이동하게 하는 과정에서 조건에 따라 useNavigate을 호출한다.

# 3. useLocation

경로 정보를 담고 있는 객체를 리턴한다.

현재 URL의 전체 구조를 알고 싶거나, 쿼리 파싱 시 자주 사용된다.

```jsx
function ProductDetail(props) {
  const location = useLocation();
  console.log(location);

  return( ... );
}

// 결과 값
{
  pathname: '/product/1',
  search: '',
  hash: '',
  state: null,
  key: 'default'
}
```

- pathname: 현재 경로 값
- search: 현재 경로의 query parameter 값

pathname은 현재 URL이 http://localhost:3000/product/1004 라고 했을 때,

query parameter을 제외한 /product/1004가 출력된다.

search는 pathmame이 출력했던 부분을 제외한 query parameter가 출력된다.

# 4. useMatch

현재 경로가 특정 패턴과 일치하는지 확인하고 그 여부를 반환한다.

컴포넌트가 렌더링된 시점의 url이 인자 안의 url과 동일한지 체크한다.

동일하다면 정보를 담은 객체를, 동일하지 않다면 null을 반환한다.

현재 URL이 특정 경로에 정확히, 혹은 일부 매칭되는지 확인하고 싶을 때 유용한다.

```jsx
const match = useMatch("/products/:productId");
if (match) {
  console.log(match.params.productId); // 매칭된 productId 출력
}

--------
// 일치할 경우 결과 값
{
  params: {...}
  pathname: "/product"
  pathnameBase: "/product"
  pattern: {caseSensitive: false, end: true, path: "/product"}
}

// 일치하지 않을 경우 결과 값
null
```

# 5. useSearchParams

URL?key=value 형태의 쿼리 파라미터를 읽고 쓰는 데 사용한다.

주로 검색 필터, 페이징, 탭 전환 같은 UI 상태를 URL에 저장하고 싶을 때,

URL 공유 시 상태가 유지되도록 만들고 싶을 때,

뒤로가기/앞으로 가기 로 상태를 복원하고 싶을 때 사용한다.

- 문법
  `const [searchParams, setSearchParams] = useSearchParams();`
  - searchParams : URLSearchParams 객체(읽기 전용)
  - setSearchParams: 쿼리 파라미터를 수정/추가하는 함수
- 주요 메서드
  ```jsx
  searchParams.get("key"); // 특정 쿼리 값 가져오기
  searchParams.has("key"); // 특정 쿼리 존재 여부 확인
  searchParams.toString(); // 전체 쿼리 문자열
  setSearchParams({ key: value }); // 쿼리 파라미터 설정
  ```
  setSearchParams() 는 새 쿼리를 설정하고 URL을 갱신한다. (브라우저 뒤로 가기 가능하도록 히스토리에 push됨)
  전체 쿼리가 대체되는 특징을 가지므로 기존 값을 유지하려면 기존 값을 복사해서 써야 한다.
  또한, searchParams.get()의 반환 값은 항상 문자열이기 때문에 숫자로 쓰고 싶으면 Numer() 등으로 변환해야 한다.
- 예시

```jsx
/search?keyword=react&page=2

---------------------------------

searchParams.get(”keyword”); // “react”

searchParams.get(”page”); // “2”
```
