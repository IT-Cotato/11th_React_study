- Memoization이란?
    
    비용이 많이 드는 함수 호출의 결과를 저장하고 동일한 입력이 다시 발생할 때 캐시된 결과를 반환하여 컴퓨터 프로그램의 속도를 높이는 데 주로 사용되는 최적화 기술이다.
    
    즉, 이전에 계산한 값을 저장하고 동일한 입력이 들어오면 다시 계산하지 않고 저장된 값을 반환하는 기법이다. 
    
- Memoization 동작방식
    1. 입력값을 확인
    2. 이전과 동일한 값인지 확인
        - 동일한 값이라면 → 저장된 값을 반환 (캐시 활용)
        - 변경되었다면 → 계산 후 결과를 저장하고 반환

- useMemo
    
    값을 메모이제이션해서 불필요한 연산을 방지해준다.
    
    ```jsx
    import React, { useState, useMemo } from 'react';
    
    const ExpensiveCalculation = ({ num }) => {
      const result = useMemo(() => {
        console.log('계산 중...');
        return num * 2;
      }, [num]);
    
      return <div>결과: {result}</div>;
    };
    ```
    
    이렇게 useMemo로 감싸서 사용하고 의존성 배열도 추가 가능하다. 
    
    위의 경우에는 num의 값이 변경되지 않는다면 ExpensiveCalculation은 연산을 또 하지 않는다.
