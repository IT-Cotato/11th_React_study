# 다크모드 구현의 패러다임: 상태와 사이드 이펙트  

## ✅ 개요

다크모드는 단순한 UI 토글 기능이 아니다.  
React에서 다크모드를 구현할 때는 **상태 관리(state)**와 **사이드 이펙트(side effect)**라는 두 축을 명확히 구분하고 제어해야 한다.  

이 문서에서는 다음 두 가지 구현 패러다임을 중심으로 다크모드의 개념적 구조를 분석한다:

- `useEffect` 기반 (반응형 선언 패러다임)
- `toggle` 함수 기반 (명령형 실행 패러다임)

---

## ✅ 핵심 개념 분석

### 1. 상태 관리 (State Management)
- 다크모드의 on/off 여부를 나타내는 상태 변수
- 예: `const [isDarkMode, setIsDarkMode] = useState(false);`

### 2. 사이드 이펙트 처리 (Side Effect)
- 상태 변경과 동시에 수행해야 하는 부가 작업
  - DOM 조작 (ex. `document.documentElement.classList`)
  - localStorage 저장
  - 시스템 테마 감지

---

## ✅ 두 접근법의 패러다임 차이

| 항목 | useEffect (반응형) | toggle 함수 (명령형) |
|------|--------------------|----------------------|
| 실행 트리거 | 상태 변화 감지 | 사용자의 명시적 호출 |
| 처리 구조 | 선언형 ("상태가 바뀌면 이 작업") | 명령형 ("이 작업을 수행하라") |
| 적합한 용도 | 내부 변화에 따른 반응 | 사용자 인터랙션 처리 |
| 예시 | localStorage 저장, DOM class 변경 | 토글 버튼 클릭 |

---

## ✅ 실제 적용 시나리오별 판단 기준

### 1. **초기 테마 설정**
- ✅ 적합한 방식: `useEffect`
- 이유: 로컬스토리지와 시스템 설정을 기반으로 테마를 초기화하는 **일회성 자동 로직**

```jsx
useEffect(() => {
  const saved = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  setIsDarkMode(saved === 'dark' || (!saved && prefersDark));
}, []);
```

### 2. **시스템 테마 변경 감지**
- ✅ 적합한 방식: `useEffect` + 미디어 쿼리 리스너

```jsx
useEffect(() => {
  const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
  const handleChange = (e) => setIsDarkMode(e.matches);
  
  mediaQuery.addEventListener('change', handleChange);
  return () => mediaQuery.removeEventListener('change', handleChange);
}, []);
```

### 3. **사용자 토글 버튼 클릭**
- ✅ 적합한 방식: `toggleDarkMode` 함수

```jsx
const toggleDarkMode = () => {
  setIsDarkMode(prev => !prev);
  // 필요한 경우 이 안에서 부수효과도 같이 수행 가능
};
```

### 4. **상태 변화에 따른 일관된 UI 반영**
- ✅ 적합한 방식: useEffect
```jsx
useEffect(() => {
  document.documentElement.classList.toggle('dark', isDarkMode);
  localStorage.setItem('theme', isDarkMode ? 'dark' : 'light');
}, [isDarkMode]);
```
### 두 접근법의 조화

다크모드처럼 사용자 인터랙션과 시스템 상태를 모두 고려해야 하는 기능에서는 두 방식의 조합이 가장 효과적이다.
- `toggleDarkMode()`는 **사용자의 의도를 명시적으로 반영**하는 트리거 역할
- `useEffect()`는 **상태 변화에 따라 부수작업을 선언적으로 처리**

이러한 분리와 조합을 통해 다음과 같은 장점이 있다:
- 관심사의 분리: 상태 vs 효과
- 재사용성과 가독성 향상
- 유지보수 시 각 부분의 책임이 명확함

### 정리
- React에서 다크모드는 단순한 UI가 아닌, 상태 추적과 사이드 이펙트 관리의 설계 문제이다.
- `useEffect`는 상태 변화에 수동적으로 반응하는 선언적 구조이다.
- **toggle 함수**는 사용자의 의도를 능동적으로 처리하는 명령적 구조이다.
- 이 둘을 조화롭게 사용하는 것이 복잡한 상태 기반 기능 구현의 핵심이다.