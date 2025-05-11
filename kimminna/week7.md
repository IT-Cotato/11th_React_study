# TDD란?

소프트웨어 개발 방법론 중 하나로, 개발자가 실제 코드를 작성하기 전에 테스트를 작성하는 것을 강조하는 개발 방법론. 

TDD의 주요 개념은 테스트를 먼저 작성하고, 그 후에 테스트를 통과할 수 있는 최소한의 코드를 작성하는 것이다.

## TDD를 왜 해야 할까?

1. 많은 기능을 테스트하기에 소스 코드에 안정감이 부여된다.
2. 실제 개발하면서 많은 시간이 소요되는 부분은 디버깅 부분이기 때문에 TDD를 사용하면 디버깅 시간이 줄고 실제 개발 시간도 줄어들게 된다.
3. 소스 코드 하나하나를 더욱 신중하게 짤 수 있기 때문에 깨끗한 코드가 나올 확률이 높다. 

# React Testing Library

React Testing Library는 React 애플리케이션의 UI 테스트를 위한 도구다. 사용자의 관점에서 애플리케이션을 테스트하는 것을 강조한다. 즉, 실제 사용자가 상호작용하는 방식으로 컴포넌트를 테스트하고 그 결과를 평가한다.

사용자 입장에서는 각 컴포넌트들이 어떻게 연결되고 props가 어떻게 전달되는지는 전혀 중요하지 않다. 어떻게 컴포넌트들이 구현되어서 사용자가 사용할지에 초점을 맞춘 것이 React Testing Library의 철학이다.

![image.png](attachment:19e8a083-7706-4ae5-a1ba-42688b9a3e28:image.png)

React Testing Library는 리액트 구성 요소 작업을 위한 API를 추가하여 DOM Testing Library 위에 구축된다. DOM Testing Library는 DOM 노드를 테스트하기 위한 매우 가벼운 솔루션이다. 

CRA로 리액트 앱을 생성하면 기본적으로 테스팅할 때 React Testing Library를 사용하지만 Vite는 그렇지 않다. 따라서 npm을 통해 추가해야 한다.

`npm install —save-dev @testing-library/react`

## 테스팅 프레임워크 - Jest vs Vitest

TDD를 진행하면서 테스팅 프레임워크로는 주로 Jest와 Vitest를 많이 쓰는데, Vite를 사용하는 경우라면 Jest보다는 Vitest를 사용하는 게 일반적이고 추천된다. 

### 왜 Vitest를 쓰는 게 좋은가?

- Vite 기반 프로젝트에 최적화됨. vite.config에서 바로 연동 가능
- 테스트 실행 속도 빠름
- Jest와 API가 거의 동일해서 익숙한 방식으로 테스트 가능
- React Testing Library와 완벽하게 호환
- UI 모드를 지원해서, 테스트 결과 및 테스트 코드를 작성하는 개발자의 DX를 향상시킬 수 있다.

| 특징 | Jest | Vitest |
| --- | --- | --- |
| **속도** | 느림 (런타임 빌드 포함) | 매우 빠름 (ESM 및 온디맨드 로딩) |
| **설정 난이도** | 복잡 (`jest.config.js` 필요) | 간단 (Vite 설정 활용) |
| **ESM 지원** | 제한적 | 완벽 지원 |
| **Mocking** | 강력한 Mocking API 제공 | Jest와 유사하지만 아직 제한적 |
| **Snapshot Testing** | 기본 제공 | 제한적 (추가 설정 필요) |
| **플러그인/생태계** | 방대함 | 상대적으로 부족 |
| **React 테스트** | 성숙하고 안정적 | 빠르고 유연함 |

# 테스트 진행 과정

![image.png](attachment:56e4f672-2301-4b65-ae99-50075ba0bfe0:image.png)

1. 테스트 코드를 작성한 뒤 npm test로 테스트를 실행해 본다. 
2. 기능이 구현되지 않은 상태이기 때문에 당연히 fail
3. 테스트 코드를 통과하기 위한 실제 코드 작성
4. 작성한 코드가 테스트를 통과하는지 확인 
5. 코드 리팩토링 

## App.test.js

```jsx
import { fireEvent, render, screen } from "@testing-library/react";
import App from "./App";

test("the counter starts at 0", () => {
  render(<App />);
  const counterElement = screen.getByTestId("counter");
  expect(counterElement).toHaveTextContent(0);
});
```

test()의 첫 번째 인자에는 test에 대한 설명, 두 번째 인자로는 실행할 test가 들어간다. 

- **render 함수**
    
    DOM에 컴포넌트를 렌더링하는 함수
    
    인자로 렌더링할 React 컴포넌트가 들어간다. 
    
    RTL에서 제공하는 쿼리 함수와 기타 유틸리티 함수를 담고 있는 객체를 리턴한다. 
    
- **query 함수**
    
    https://testing-library.com/docs/queries/about/
    
    query는 페이지에서 요소를 찾기 위해 테스트 라이브러리가 제공하는 방법이다. 여러 유형의 쿼리가 있는데, 이들 간의 차이점은 요소가 발견되지 않으면 오류를 발생하는지, 아니면 Promise를 반환하고 다시 시도하는지의 여부이다. 각각의 content에 따라 특정 쿼리가 더 유용할 수 있다. 
    
    - get, query, find 간의 차이점
        
        ![image.png](attachment:3b656a24-61a9-43ef-bba7-4d515c9e2d1d:image.png)
        
        ![image.png](attachment:1b33f018-7ba6-4bbc-916d-cfd334dde447:image.png)