# React 프로젝트 기초 세팅 및 상태관리 정리

---

## 1. 폰트 세팅

Google Fonts에서 원하는 글꼴을 프로젝트 전역에 적용하는 방식

이번 프로젝트에서는 `Nanum Pen Script`, `Yeon Sung` 사용함

- Google Fonts → `@import` 코드 복사하여 `App.css` 상단에 삽입
    
    ```css
    @import url('https://fonts.googleapis.com/css2?family=Nanum+Pen+Script&family=Yeon+Sung&display=swap');
    ```
    
- `font-family` 속성으로 원하는 폰트 적용
    
    ```css
    body {
      font-family: 'Nanum Pen Script', cursive;
      font-family: 'Yeon Sung', system-ui; /* 이 폰트 적용 */
    }
    ```
    
- 여러 폰트를 사용할 경우, CSS 상에서 아래에 있는 폰트가 실제 적용됨
- 한 줄에 여러 폰트가 있을 경우, 가장 왼쪽 폰트부터 우선적으로 적용됨

---

## 2. 전체 레이아웃 세팅

모든 페이지에 공통적으로 적용되는 기본 레이아웃 스타일 정의 작업

- 페이지를 중앙에 배치하고, 기본 배경과 박스 그림자 등을 설정
    
    ```css
    body {
      background-color: #f6f6f6;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      font-family: 'Nanum Pen Script';
    }
    
    #root {
      background-color: white;
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
    }
    
    .App {
      padding: 0 20px;
      min-height: 100vh;
    }
    ```
    

페이지 구성의 일관성을 유지하고 반응형 구조를 만들기 위한 설정

- `media query`를 사용하여 화면 크기에 따라 `.App`의 너비 조정
    - 650px 이상이면 고정 폭 640px
    - 650px 이하면 화면 너비의 90% 차지
    
    ```css
    @media (min-width: 650px) {
      .App {
        width: 640px;
      }
    }
    
    @media (max-width: 650px) {
      .App {
        width: 90vw;
      }
    }
    ```
    

---

## 3. 이미지 에셋 세팅

공통으로 사용하는 감정 이미지 등의 정적 파일을 효율적으로 관리하기 위한 구조 세팅

- `public/assets` 폴더에 감정 이미지 저장
- React 내부에서 `process.env.PUBLIC_URL`을 활용하여 이미지 경로 지정
- public 폴더 기준으로 정적 파일을 불러오는 방식
    
    ```jsx
    <img src={process.env.PUBLIC_URL + '/assets/emotion1.png'} alt="emotion1" />
    <img src={process.env.PUBLIC_URL + '/assets/emotion2.png'} alt="emotion2" />
    ```
    

→ 컴포넌트 어디서든 정적 이미지 경로를 안전하게 불러올 수 있음

---

## 4. 공통 컴포넌트 만들기

### MyButton

자주 반복되는 버튼 요소를 컴포넌트로 분리하여 재사용 가능하게 구성

- `text`, `type`, `onClick` props를 받음
- `type`은 `'positive'`, `'negative'`, `'default'` 중 하나로 지정 가능
- `type` 값에 따라 스타일이 달라짐
    
    ```jsx
    const MyButton = ({ text, type, onClick }) => {
      const btnType = ['positive', 'negative'].includes(type) ? type : 'default';
    
      return (
        <button className={['MyButton', `MyButton_${btnType}`].join(' ')} onClick={onClick}>
          {text}
        </button>
      );
    };
    
    // 기본 타입 지정
    MyButton.defaultProps = {
      type: 'default',
    };
    
    export default MyButton;
    ```
    
    사용 예시
    
    ```jsx
    <MyButton text="작성완료" type="positive" onClick={() => alert('작성')} />
    <MyButton text="삭제하기" type="negative" onClick={() => alert('삭제')} />
    <MyButton text="수정하기" onClick={() => alert('수정')} />
    ```
    

### MyHeader

모든 화면 상단에 공통으로 사용하는 헤더 컴포넌트

- `leftChild`, `headText`, `rightChild` 3개의 props를 받아 구성
- 왼쪽 버튼, 중앙 텍스트, 오른쪽 버튼으로 나뉘며, 각각 위치 지정
- 버튼 컴포넌트를 직접 props로 전달하여 재사용성 향상됨
    
    ```jsx
    const MyHeader = ({ leftChild, headText, rightChild }) => {
      return (
        <header>
          <div className="head_btn_left">{leftChild}</div>
          <div className="head_text">{headText}</div>
          <div className="head_btn_right">{rightChild}</div>
        </header>
      );
    };
    
    export default MyHeader;
    ```
    
    사용 예시
    
    ```jsx
    <MyHeader
      headText="일기장"
      leftChild={<MyButton text="뒤로" onClick={() => navigate(-1)} />}
      rightChild={<MyButton text="저장" type="positive" onClick={() => alert('저장')} />}
    />
    ```
    

---

## 5. 상태 관리 (useReducer)

복잡한 상태 변화를 명확하게 관리하기 위한 reducer 기반 상태 처리 방식

- `useReducer`로 상태 로직 분리
- 액션 타입은 `INIT`, `CREATE`, `REMOVE`, `EDIT`
- 각 액션마다 새로운 상태 배열 반환
- ID는 `useRef`를 사용하여 중복되지 않게 관리함
    
    ```jsx
    const reducer = (state, action) => {
      switch (action.type) {
        case 'INIT':
          return action.data; // 초기값 설정
        case 'CREATE':
          return [action.data, ...state]; // 새 일기 추가
        case 'REMOVE':
          return state.filter((it) => it.id !== action.targetId); // 일기 삭제
        case 'EDIT':
          return state.map((it) => (it.id === action.data.id ? { ...action.data } : it)); // 일기 수정
        default: 
          return state;
      }
    }
    ```
    
    App 컴포넌트에서 사용
    
    ```jsx
    const [data, dispatch] = useReducer(reducer, []);
    const dataId = useRef(0); // 고유 ID 관리
    
    // 생성
    const onCreate = (date, content, emotion) => {
      dispatch({
        type: 'CREATE',
        data: {
          id: dataId.current,
          date: new Date(date).getTime(),
          content,
          emotion,
        },
      });
      dataId.current += 1;
    };
    ```
    

상태 변경 흐름을 하나의 reducer로 통합하여 예측 가능하고 테스트 가능한 코드 구조를 만듦 (useState보다 구조적)

---

## 6. 전역 상태 공유 (Context API)

컴포넌트 트리 전체에서 상태값 및 상태 변경 함수를 공유하기 위한 구조

- `DiaryStateContext`: 상태 값(data)을 하위 컴포넌트에 전달함
- `DiaryDispatchContext`: 상태 변경 함수들(onCreate, onEdit, onRemove)을 전달함
- 2개의 Context를 분리하여 관리 → 역할 구분 명확함
- `useContext()` 훅을 통해 필요한 컴포넌트에서 바로 접근 가능

Context 생성

```jsx
export const DiaryStateContext = React.createContext(); // 데이터 상태
export const DiaryDispatchContext = React.createContext(); // 상태 변경 함수
```

Provider 적용

```jsx
<DiaryStateContext.Provider value={data}>
  <DiaryDispatchContext.Provider value={{ onCreate, onRemove, onEdit }}>
    <BrowserRouter>
      <Routes>...</Routes>
    </BrowserRouter>
  </DiaryDispatchContext.Provider>
</DiaryStateContext.Provider>
```

하위 컴포넌트에서 사용 예시

```jsx
// 상태 가져오기
const diaryList = useContext(DiaryStateContext);

// 함수 가져오기
const { onCreate, onEdit, onRemove } = useContext(DiaryDispatchContext);
```

props drilling 없이 필요한 곳에서만 상태를 불러와 사용하도록 도와주는 구조
