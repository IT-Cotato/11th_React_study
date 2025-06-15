# Immer & useImmer

## 불변성 유지 없이도 직관적으로 상태를 수정할 수 있게 도와주는 라이브러리
- 리액트에서 state는 직접 수정하면 안 되고 반드시 새로운 객체로 바꿔야 함 (불변성 유지 필요)
- Immer는 이 불편한 과정을 Proxy 기반으로 자동 처리
- 복잡한 중첩 객체를 다룰 때 유용
- useImmer는 React의 useState처럼 사용 가능

---

## 왜 필요한가?

| 기존 방식 (불변성 유지 수동) | Immer 사용 시 |
| ------------------------ | -------------- |
| 객체 복사/스프레드로 수정 필요 | 직접 수정하듯 작성하면 내부적으로 불변성 유지됨 |
| 코드 길고 실수 잦음 | 코드 직관적이고 간결함 |
| 중첩 깊을수록 복잡도 증가 | 깊이 상관없이 직관적 |

---

## 기본 사용법 (JS만 사용할 경우)

```jsx
import { produce } from 'immer';

const nextState = produce(originalState, draft => {
  draft.user.name = 'Jimin';
});
```

- produce(original, updater) 구조
- draft는 Proxy 객체로, 직접 수정하듯 작성 가능

---

## React에서 useImmer 사용

```jsx
import { useImmer } from 'use-immer';

const [person, updatePerson] = useImmer({
  name: 'Jimin',
  profile: {
    age: 24,
    job: 'Developer',
  },
});

updatePerson(draft => {
  draft.profile.age += 1;
});
```

- useState 대신 useImmer 사용
- setState 대신 updatePerson 사용
- 불변성 유지 신경 X → 직관적인 코드

---

## 실무 팁

- useImmer는 state 복잡도 높을 때만! 단순한 경우엔 useState로도 충분
- useReducer + Immer 조합도 가능 (복잡한 로직, 액션 기반 업데이트에 유용)
- Zustand, Redux Toolkit 등도 내부적으로 Immer 사용

---

## 다른 라이브러리들과 비교

| 목적 | Immer | Zustand + Immer | Redux Toolkit |
| ---- | ------ | ---------------- | -------------- |
| 불변성 유지 | O (자동) | O | O |
| 사용 방식 | produce / useImmer | Hook으로 상태 관리 + Immer 미들웨어 | createSlice 내부에서 Immer 자동 적용 |
| 학습 난이도 | 낮음 | 낮음 | 중간 |

---

## 추천 레퍼런스

- [Immer 공식문서](https://immerjs.github.io/immer/)
- [use-immer GitHub](https://github.com/immerjs/use-immer)
- [Redux Toolkit (Immer 내장)](https://redux-toolkit.js.org/)
