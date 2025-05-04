# CSS 전처리기 & 후처리기 개요

## 🔧 전처리기 (Preprocessors)

전처리기는 **CSS가 브라우저에서 사용되기 전, 개발자가 작성한 스타일을 더 강력한 문법으로 확장**해주는 도구입니다. 결과적으로 전처리기 코드는 일반 CSS로 컴파일되어 브라우저에 전달됩니다.

### 대표적인 전처리기

- **Sass/SCSS**: 가장 널리 사용됨. 변수, 믹스인, 중첩 등 다양한 문법 지원
- **Less**: Sass와 유사하지만 JavaScript 기반 문법을 포함
- **Stylus**: 자유로운 문법, 옵션이 많고 유연하지만 학습 곡선이 있음

---

### 🔍 Sass와 SCSS 차이

Sass는 두 가지 문법을 제공합니다:

| 문법            | 확장자  | 스타일        | 특징                                   |
| --------------- | ------- | ------------- | -------------------------------------- |
| Sass (original) | `.sass` | 들여쓰기 기반 | 중괄호 `{}`와 세미콜론 `;` 없음        |
| SCSS            | `.scss` | CSS 유사      | 기존 CSS와 100% 호환됨, 가장 널리 쓰임 |

### 예시 비교

#### ✅ SCSS 문법 (`.scss`)

```scss
$primary-color: #3498db;

.button {
  background-color: $primary-color;
  &:hover {
    background-color: darken($primary-color, 10%);
  }
}
```

#### ✅ Sass 문법 (`.sass`)

```sass
$primary-color: #3498db

.button
  background-color: $primary-color
  &:hover
    background-color: darken($primary-color, 10%)
```

> 대부분의 프로젝트에서는 SCSS 문법을 사용하며, Sass 문법은 간결함을 선호할 때 선택됩니다.

---

## ⚙️ 후처리기 (Postprocessors)

후처리기는 **완성된 CSS를 변형하거나 최적화**하는 도구입니다.  
대표적으로 **PostCSS**가 있으며, 다양한 플러그인을 조합해 원하는 기능을 수행할 수 있습니다.

### 대표적인 기능 (PostCSS + 플러그인)

- **Autoprefixer**: 브라우저 호환을 위한 접두사 자동 추가
- **cssnano**: CSS 파일 크기 최소화 (minify)
- **Tailwind CSS**: 실질적으로 PostCSS 위에 구축됨
- **env() 사용 등 최신 문법 지원**

### 예시: Autoprefixer

입력:

```css
.button {
  display: flex;
}
```

출력:

```css
.button {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
}
```

---

## ✅ 전처리기 vs 후처리기 비교

| 구분      | 전처리기 (Sass, Less 등) | 후처리기 (PostCSS 등)              |
| --------- | ------------------------ | ---------------------------------- |
| 적용 시점 | 작성 전                  | 작성 후                            |
| 주요 목적 | 문법 확장, 구조화        | 자동 최적화, 호환성 향상           |
| 대표 기능 | 변수, 중첩, 믹스인       | 접두사 추가, minify, 최신 CSS 지원 |
| 도구 예시 | Sass, Less, Stylus       | PostCSS, Autoprefixer              |

---

## 📝 마무리

- **전처리기**는 더 나은 작성 경험을,
- **후처리기**는 더 나은 배포 결과물을 위한 도구입니다.

React 환경에서는 보통 전처리기로는 Sass(SCSS 문법), 후처리기로는 PostCSS + Autoprefixer 조합이 가장 흔하게 사용됩니다.
