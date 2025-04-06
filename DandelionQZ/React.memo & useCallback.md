React.memo는 컴포넌트의 불필요한 리렌더링을 방지하는 고차 컴포넌트(HOC)이다.

props가 변경되지 않으면, 이전 렌더링 결과를 재사용하여 성능을 최적화할 수 있다.

- 사용예시
    
    ```jsx
    import React, { useState } from 'react';
    
    // React.memo 적용된 자식 컴포넌트
    const ChildComponent = React.memo(({ text }) => {
      console.log('Component 렌더링');
      return <div>{text}</div>;
    });
    ```
    
    이렇게 리렌더링이 필요없는 컴포넌트를 React.memo로 감싸주면 된다.
    

그런데 React.memo를 사용했더라도 함수를 전달하게 되면 메모리에 새로운 함수가 할당되어 props가 변경된 것으로 인식된다. → 리렌더링 된다.

이를 방지하기 위해서는 useCallback을 사용할 수 있다. 

```jsx
import React, { useState, useCallback } from 'react';

const handleClick = useCallback(() => {
    console.log('버튼 클릭!');
  }, []); // 참조하는 값이 있다면 의존성 배열에 추가 가능 
```

이렇게 useCallback으로 감싸서 함수를 작성하면 동일한 참조값을 사용하여 필요하지 않은 리렌더링이 발생하지 않는다.
