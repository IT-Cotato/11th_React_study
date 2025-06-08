## 1. `Home.jsx` – 월별 일기 필터링

```jsx
// 현재 기준 날짜 상태 (처음에는 오늘 날짜)
const [pivotDate, setPivotDate] = useState(new Date());
```

- `pivotDate`: 현재 기준 날짜 (기본값은 오늘)
- 화면 상단에 "2025년 6월"처럼 표시되며, 이 값을 기준으로 일기 필터링

---

```jsx
// 다음 달로 이동하는 함수
const onIncreaseMonth = () => {
  setPivotDate(
  new Date(
	  pivotDate.getFullYear(),   // 현재 연도 유지
	  pivotDate.getMonth() + 1)  // 다음 달로 이동
  );
};
```

- 현재 월을 1 증가시킨 새 `Date` 객체를 만들고 상태 업데이트
- 예: 2025년 6월 → 2025년 7월

---

```jsx
// 이전 달로 이동하는 함수
const onDecreaseMonth = () => {
  setPivotDate(
  new Date(
	  pivotDate.getFullYear(),  // 현재 연도 유지
	  pivotDate.getMonth() - 1  // 이전 달로 이동
	  )
	);
};
```

- 현재 월을 1 감소시킨 새 `Date` 객체를 만들고 상태 업데이트
- 예: 2025년 6월 → 2025년 5월

---

```jsx
// 특정 날짜(pivotDate) 기준으로 해당 월의 일기만 필터링하는 함수
const getMonthlyData = (pivotDate, data) => {

  const beginTime = new Date(
    pivotDate.getFullYear(),          // 현재 연도
    pivotDate.getMonth(),             // 현재 월 (0~11)
    1                                 // 1일 00:00:00
  ).getTime();

  const endTime = new Date(
    pivotDate.getFullYear(),
    pivotDate.getMonth() + 1,         // 다음 달의 0일 → 이번 달 마지막 날
    0,
    23, 59, 59                        // 23:59:59
  ).getTime();
```

- `beginTime`: 해당 월의 1일 00시 00분
- `endTime`: 해당 월의 마지막 날 23시 59분

---

```jsx
  return data.filter((item) =>
    beginTime <= item.createdDate && item.createdDate <= endTime
  );
  // 해당 월의 범위 안에 작성된 일기만 추출
};
```

- `data` 배열에서 일기들의 작성일(createdDate)이 범위에 속하는 것만 필터링
- 즉, 해당 월에 작성된 일기만 추출됨

---

## 2. `DiaryList.jsx` – 정렬 기능

```jsx
// 정렬 기준을 저장하는 상태 변수 (latest 또는 oldest)
const [sortType, setSortType] = useState("latest");
```

- `sortType` 상태로 정렬 기준을 설정 (`latest` 또는 `oldest`)
- 초기값은 `"latest"` 즉, 최신순

---

```jsx
// 드롭다운에서 정렬 기준 변경 시 실행되는 함수
const onChangeSortType = (e) => {
  setSortType(e.target.value); // 선택된 값으로 상태 업데이트
};
```

- 드롭다운 메뉴에서 선택한 값에 따라 `sortType`을 업데이트

---

```jsx
// 정렬된 데이터를 반환하는 함수
const getSortedData = () => {
  return data.toSorted((a, b) => {
    if (sortType === "oldest") {
      return a.createdDate - b.createdDate; // 오래된 순: 작은 날짜부터 정렬
    } else {
      return b.createdDate - a.createdDate; // 최신 순: 큰 날짜부터 정렬
    }
  });
};
```

- `toSorted()`는 정렬된 새 배열을 반환 (원본 배열 불변)
- `"oldest"`: 오름차순 (오래된 것부터)
- `"latest"`: 내림차순 (최근 것부터)

---

```jsx
// 정렬된 데이터를 기반으로 DiaryItem 컴포넌트를 생성
{getSortedData().map((item) => (
  <DiaryItem key={item.id} {...item} />
))}
```

- 정렬된 일기 데이터를 하나씩 `DiaryItem` 컴포넌트에 전달하여 렌더링
- `key={item.id}`: React 최적화를 위한 고유값 설정
- `{...item}`: DiaryItem에 props로 id, content, emotionId, createdDate 등 모두 전달

---

## 3. `DiaryItem.jsx` – 일기 상세/수정 페이지 이동

```jsx
// 라우팅 이동을 위한 훅
const nav = useNavigate();
```

- `useNavigate()` 훅: 버튼 클릭으로 라우팅을 이동시킬 때 사용

---

```jsx
// 상세 페이지로 이동하는 함수
const goDiaryPage = () => {
  nav(`/diary/${id}`); // 예: /diary/3
};
```

- 일기 카드 클릭 시 → `/diary/해당ID` 경로로 이동
- 예: id가 3이라면 `/diary/3`

---

```jsx
// 수정 페이지로 이동하는 함수
const goEditPage = () => {
  nav(`/edit/${id}`); // 예: /edit/3
};
```

- "수정하기" 버튼 클릭 시 `/edit/3`처럼 수정 화면으로 이동

---

```jsx
// 감정 이미지 출력 (emotionId에 따라 이미지 다름)
<img src={getEmotionImage(emotionId)} alt="emotion" />
```

- 감정 ID에 따라 대응되는 이미지 파일을 렌더링
- `emotionId`가 1~5이면 각각 다른 표정의 이미지 출력

---

```jsx
// 작성 날짜 출력 (yyyy.mm.dd 형태)
<div className="created_date">
  {new Date(createdDate).toLocaleDateString()}
</div>
```

- `toLocaleDateString()`을 사용해 사람이 읽기 편한 날짜 형식으로 표시

---

## 4. `Diary.jsx`, `Edit.jsx` – URL 파라미터 처리

```jsx
// URL의 :id 값을 추출하는 훅
const params = useParams();
```

---

```jsx
// 추출된 ID를 활용해 해당 일기를 불러오거나 출력
return <div>{params.id}번 일기입니다</div>;
```

- `params.id`: URL 경로에서 가져온 숫자 (예: /diary/3 → 3)

---

## 5. `Header.jsx` – 공통 헤더 컴포넌트

```jsx
const Header = ({ title, leftChild, rightChild }) => {
  return (
    <header className="Header">
      <div className="header_left">{leftChild}</div>
      <div className="header_center">{title}</div>
      <div className="header_right">{rightChild}</div>
    </header>
  );
};
```

- `leftChild`: 왼쪽 버튼 (예: 이전 달)
- `title`: 중앙 제목 텍스트 (예: 2025년 6월)
- `rightChild`: 오른쪽 버튼 (예: 다음 달)

→ 어떤 페이지든 유연하게 재사용 가능한 구조
