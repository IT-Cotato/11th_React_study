localStorage는 브라우저에서 제공하는 저장 공간으로, 사용자의 데이터를 브라우저에 영구적으로 저장할 수 있다.

React에서는 localStorage를 사용하여 상태를 유지하거나, 사용자 데이터를 저장하는 데 활용할 수 있다.

- 데이터 저장
    
    ```jsx
    localStorage.setItem('key', 'value');
    ```
    
- 데이터 가져오기
    
    ```jsx
    const data = localStorage.getItem('key');  // 'value'
    ```
    
- 데이터 삭제하기
    
    ```jsx
    localStorage.removeItem('key');
    
    // 모든 데이터 삭제
    localStorage.clear();
    ```
