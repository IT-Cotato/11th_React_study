HOC은 Higher-Order Component로 ‘고차 컴포넌트’라는 의미이다.

컴포넌트를 인자로 받아서 새로운 컴포넌트를 반환하며, 재사용성과 코드 중복 제거를 위해 사용된다.

재사용 가능한 로직만을 분리해서 컴포넌트로 만들고 재사용이 불가능한 부분은 파라미터로 받아서 처리한다.

- HOC 함수 정의
    
    ```jsx
    import React from "react";
    
    // HOC: 인증이 필요한 컴포넌트를 감싸는 함수
    function withAuth(WrappedComponent) {
      return function AuthComponent(props) {
        const isLoggedIn = true; // 로그인 여부 (실제로는 상태나 context에서 가져옴)
    
        if (!isLoggedIn) {
          return <p>로그인이 필요합니다.</p>;
        }
    
        return <WrappedComponent {...props} />;
      };
    }
    
    export default withAuth;
    ```
    
    - `WrappedComponent`는 감싸질 컴포넌트
    - `isLoggedIn`이 `false`면 로그인 요청 메시지를, `true`면 원래 컴포넌트를 렌더링
- HOC 적용
    
    ```jsx
    import React from "react";
    import withAuth from "./withAuth"; // HOC import
    
    function Dashboard() {
      return <h2>대시보드 페이지입니다.</h2>;
    }
    
    export default withAuth(Dashboard);
    ```
    
    - `Dashboard` 컴포넌트에 `withAuth`를 적용하면 로그인 상태에 따라 다른 UI가 출력
- 사용 예시
    
    ```jsx
    import React from "react";
    import Dashboard from "./Dashboard";
    
    function App() {
      return (
        <div>
          <Dashboard user="John" />
        </div>
      );
    }
    
    export default App;
    ```
    
- 단점
    - 컴포넌트 트리가 깊어질 수 있음 → React DevTools에서 디버깅이 어려워짐.
    - Hook이 등장하면서 HOC 사용이 줄어듦 → useAuth 같은 Custom Hook을 사용하는 경우가 많음.

- Hook을 이용한 대체 방식
    
    ```jsx
    function useAuth() {
      const isLoggedIn = true;
      return isLoggedIn;
    }
    
    function Dashboard() {
      const isLoggedIn = useAuth();
    
      if (!isLoggedIn) {
        return <p>로그인이 필요합니다.</p>;
      }
    
      return <h2>대시보드 페이지입니다.</h2>;
    }
    
    ```
    

| 방식 | 장점 | 단점 | 사용 예시 |
| --- | --- | --- | --- |
| **조건부 렌더링 (`if`)** | 코드가 직관적이고 간단 | 여러 곳에서 같은 로직이 반복될 가능성이 있음 | 특정 페이지에서만 인증 체크 |
| **HOC (`withAuth`)** | 여러 컴포넌트에서 공통 로직을 재사용 가능 | 컴포넌트 트리가 깊어질 수 있음, 가독성이 떨어질 수 있음 | 인증, 로깅, 데이터 페칭 같은 고유한 기능 추가 |
| **Hook (`useAuth`)** | 함수형 컴포넌트에서 쉽게 재사용 가능, 깔끔한 코드 | 클래스형 컴포넌트에서는 사용 불가능 (React 16.8 이상 필요) | Context API, 사용자 인증, 상태 관리 |

HOC와 Hook의 목적은 “공통 로직을 효율적으로 재사용하는 것”에 있다.
