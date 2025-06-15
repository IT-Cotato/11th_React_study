## 1. 헤더 구현

```jsx
<MyHeader
  headText="새로운 일기 쓰기"
  leftChild={<MyButton text="< 뒤로가기" onClick={() => navigate(-1)} />}
/>
```

- 제목 고정
- `useNavigate()` 의 반환값으로 받은 함수의 매개변수로 '-1' 을 넣어서 이전 페이지로 이동

---

## 2. 시간 선택

- 달력을 띄워 날짜를 선택하는 입력창 구현

```jsx
const getStringDate = (date) => date.toISOString().slice(0, 10);

const [date, setDate] = useState(getStringDate(new Date()));
```

```jsx
<input
  className="input_date"
  type="date"
  value={date}
  onChange={(e) => setDate(e.target.value)}
/>
```

- 오늘 날짜를 기본값으로 설정
- `toISOString()`은 ISO 8601 표준 포맷 문자열을 반환 (예: `"2025-06-15T04:03:20.000Z"`)
- 이 중 앞의 10자리만 잘라 `"2025-06-15"` 형태로 사용
- 시간대는 항상 UTC 기준이기 때문에, 로컬 시간 기준이 필요한 경우 `toLocaleDateString()`도 고려 가능
- 날짜 변경 시 `setDate()`로 상태 업데이트

---

## 3. 감정 선택

- 감정 데이터 배열을 만들어 props로 `EmotionItem` 컴포넌트에 전달하여 감정 목록 출력

```jsx
const emotionList = [
  { emotion_id: 1, emotion_img: "...", emotion_descript: "완전 좋음" },
  ...
  { emotion_id: 5, emotion_img: "...", emotion_descript: "끔찍함" }
];

// 환경변수 설정 - Vite나 CRA에서 PUBLIC_URL이 없으면 오류 방지
const env = process.env;
env.PUBLIC_URL = env.PUBLIC_URL || '';
```

```jsx
const [emotion, setEmotion] = useState(3);

{emotionList.map((it) => (
  <EmotionItem
    key={it.emotion_id}
    {...it}
    onClick={setEmotion}
    isSelected={it.emotion_id === emotion}
  />
))}
```

- 감정은 총 5단계
- 이미지 경로는 `process.env.PUBLIC_URL`을 사용해 상대 경로 설정
    
    → 배포 환경(static 경로)에서 이미지 로딩 실패 방지
    

### EmotionItem.js

```jsx
<div
  className={`EmotionItem ${isSelected ? `EmotionItem_on_${emotion_id}` : 'EmotionItem_off'}`}
  onClick={() => onClick(emotion_id)}
>
  <img src={emotion_img} />
  <span>{emotion_descript}</span>
</div>
```

- 선택된 감정은 `isSelected`로 스타일 적용
- 클릭 시 상위 컴포넌트의 `onClick()` 호출
- 선택된 감정은 색상 강조

---

## 4. 일기 내용 입력

```jsx
const [content, setContent] = useState('');
const contentRef = useRef();
```

```jsx
<textarea
  placeholder="오늘은 어땠나요?"
  ref={contentRef}
  value={content}
  onChange={(e) => setContent(e.target.value)}
/>
```

- 입력된 일기 내용은 `content` 상태에 저장
- 아무 것도 입력하지 않은 채 작성완료를 누르면 `ref`로 포커싱 유도

---

## 5. 작성/취소 버튼 및 제출 처리

```jsx
const { onCreate } = useContext(DiaryDispatchContext);
```

```jsx
const handleSubmit = () => {
  if (content.length < 1) {
    contentRef.current.focus();
    return;
  }

  // 수정 여부에 따라 분기 처리
  if (!isEdit) {
    onCreate(date, content, emotion);
  } else {
    onEdit(originData.id, date, content, emotion);
  }

  // 홈으로 이동, replace로 뒤로가기 막음
  navigate('/', { replace: true });
};
```

```jsx
<div className="control_box">
  <MyButton text="취소하기" onClick={() => navigate(-1)} />
  <MyButton text="작성완료" type="positive" onClick={handleSubmit} />
</div>
```

- `onCreate()` 또는 `onEdit()` 호출
- `replace: true`로 뒤로가기 막음
- 취소는 `navigate(-1)`로 처리

---

## 6. 일기수정(Edit) 페이지 연결

### Edit.js

```jsx
const { id } = useParams(); // URL의 :id 파라미터 추출
const diaryList = useContext(DiaryStateContext); // 전체 일기 목록
const [originData, setOriginData] = useState(); // 수정할 일기 데이터 저장

 // Mount 시점에 id, diaryList 가 변할 때만 꺼내옴
useEffect(() => {
  if (diaryList.length >= 1) {
    const targetDiary = diaryList.find((it) => parseInt(it.id) === parseInt(id));
    if (targetDiary) {
      setOriginData(targetDiary);
    } else {
      navigate('/', { replace: true }); // 잘못된 접근이면 홈으로 보냄
    }
  }
}, [id, diaryList]);
```

```jsx
return (
  <div>
    {originData && <DiaryEditor isEdit={true} originData={originData} />}
  </div>
);
```
