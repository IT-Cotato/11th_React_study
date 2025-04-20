# CSS 방식 비교: CSS Modules vs Styled Components vs Tailwind CSS

## 6.3 CSS Modules

- 파일별로 모듈성을 갖춘 방식
- `.module.css` 파일을 통해 컴포넌트 단위로 스타일을 캡슐화
- 클래스 이름 충돌 방지 → 각 클래스는 해시값으로 변환됨
- 별도 파일에서 작성하는 **Pure CSS** 기반
- 사용 예:

```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
}
```

```jsx
import styles from "./Button.module.css";

function Button() {
  return <button className={styles.button}>Click me</button>;
}
```

- ✅ 유지보수, 협업 시 안정적이며 구조적 스타일링에 유리

---

## 6.4 Styled Components

- CSS-in-JS 방식 중 하나로 **자바스크립트 파일 내부에서 스타일 작성**
- React, React Native 등 다양한 환경에서 사용 가능
- 스타일을 컴포넌트 단위로 정의할 수 있어 재사용성 높음
- JS 코드 안에서 조건부 스타일 작성이 편리함
- 단점: 런타임 오버헤드, 추적 어려움, 클래스 이름 디버깅 불편

```jsx
import styled from "styled-components";

const Button = styled.button`
  background-color: blue;
  color: white;
`;

function App() {
  return <Button>Click me</Button>;
}
```

- ⚠️ 코드가 길어지고 스타일과 로직이 섞여 복잡해질 수 있음

---

## 6.5 Tailwind CSS

- **유틸리티 클래스 기반의 CSS 프레임워크**
- 별도의 CSS 파일 없이, HTML이나 JSX 내에서 바로 클래스 적용
- 독립적이고 고립된 스타일 구성 가능 (디자인 시스템 구축 용이)
- 클래스 이름 창작의 스트레스가 없음
- 단점: 태그 내에 클래스가 몰려 가독성이 떨어질 수 있음

```jsx
function Button() {
  return (
    <button className="bg-blue-500 text-white py-2 px-4 rounded">
      Click me
    </button>
  );
}
```

- ✅ 빠른 개발에 적합하고 정해진 디자인 시스템에 강함

---

## 6.6 방식별 비교 요약

| 항목             | CSS Modules          | Styled Components             | Tailwind CSS                    |
| ---------------- | -------------------- | ----------------------------- | ------------------------------- |
| 방식             | 파일 분리 (Pure CSS) | JS 내 스타일 정의 (CSS-in-JS) | 유틸리티 클래스 직접 사용       |
| 모듈성           | ✅ 매우 높음         | ✅ 높음                       | ✅ 높음                         |
| 클래스 충돌 방지 | ✅ 자동 처리         | ✅ 자동 처리                  | ✅ 클래스 자체가 독립적         |
| 가독성           | ✅ 높음              | ⚠️ JS와 섞여 다소 낮음        | ⚠️ 클래스 몰림으로 낮을 수 있음 |
| 스타일 재사용    | ✅ 명시적 import     | ✅ 컴포넌트 자체 재사용       | ⚠️ 추상화 필요                  |

---

## 6.7 주관적 추천

| 상황                                         | 추천 방식                         |
| -------------------------------------------- | --------------------------------- |
| 큰 규모 협업 프로젝트, 첫 실무 프로젝트      | **CSS Modules**                   |
| 스타일을 자바스크립트와 함께 작성하고 싶다면 | Styled Components (단, 주의 필요) |
| 빠른 개발, 일관된 디자인 시스템 기반 개발    | **Tailwind CSS**                  |

> 🔖 스타일과 로직이 뒤엉키는 걸 최소화하고 싶다면 CSS Modules,  
> 빠르게 뷰를 구성해야 하는 사이드 프로젝트라면 Tailwind CSS가 특히 유리합니다.

## 실무 팁 및 최신 흐름

- 💡 **Styled Components**는 조건부 스타일링이나 테마 기반 스타일에서 장점이 큼
- 💡 **Tailwind CSS**는 Headless UI 등과 함께 쓰면 컴포넌트 구성에 유리
- 💡 CSS Modules는 PostCSS와 함께 쓰면 커스터마이징 유연성 증가
