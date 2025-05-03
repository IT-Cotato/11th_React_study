## ✅ 사전 체크

- [ ] 작업용 리포지토리(예: `my-project`)가 존재하고 `public` 설정됨
- [ ] GitHub Pages용 레포지토리 생성 (`깃헙ID.github.io` 형식, `public`, 모두 **소문자**)
- [ ] 로컬에서 `npm start`로 실행 시 콘솔/터미널에 에러 없음

---

## ⚙️ 배포 과정

1. **gh-pages 설치**
   ```bash
   npm install gh-pages --save-dev
   ```

2. package.json 수정
   ```json
   "homepage": "https://깃헙ID.github.io/레포이름/",
   "scripts": {
      ...
      "deploy": "gh-pages -d build"
    }
   ```

3. 빌드 및 배포
   ```bash
   npm run build
   npm run deploy
   ```
npm run deploy 명령어는 기본적으로 build 디렉토리를 생성하고, 
gh-pages 브랜치를 만들고 업로드까지 자동으로 수행한다. 
다만 build 디렉토리가 없다는 오류가 날 경우, 반드시 npm run build 먼저 실행 후 deploy 해야한다. 

4. GitHub Pages 설정
   작업 리포지토리 > Settings > Pages > Branch: gh-pages 선택 후 저장

---

## 🔄 수정 반영 방법
코드를 수정한 후 배포하려면?

1. 수정된 내용을 작업 브랜치에 커밋

2. 아래 명령어 재실행
  ```bash
  npm run build
  npm run deploy
  ```
반영에는 시간이 조금 걸릴 수 있다.

---

## 🌐 접속 주소
  ```cpp
  https://깃헙ID.github.io/레포이름
  ```

---

## ⚠️ 라우터와 새로고침 오류 해결

GitHub Pages는 정적 파일을 제공하므로, React의 **Browser Router** 사용 시 새로고침(F5)할 때 `404 Not Found` 오류가 발생할 수 있다.

### 🔧 해결법 1 : 404 리디렉션 핸들링

1. **`public/404.html` 파일 추가**
    - `var pathSegmentsToKeep = 1;` 로 설정 (기존 0 → 1 변경)
2. **`public/index.html`에 스크립트 추가**
    
    ```html
    <script type="text/javascript">
      (function (l) {
        if (l.search[1] === '/') {
          var decoded = l.search.slice(1).split('&').map(function (s) {
            return s.replace(/~and~/g, '&')
          }).join('?');
          window.history.replaceState(null, null,
            l.pathname.slice(0, -1) + decoded + l.hash
          );
        }
      }(window.location))
    </script>
    ```
3. **다시 빌드 및 배포**
    
    ```bash
    bash
    복사편집
    npm run build
    npm run deploy
    ```

### 🔧 해결법 2: BrowserRouter basename 설정

만약 **BrowserRouter**를 계속 사용하고 싶다면, `basename` 속성을 이용해 기본 경로를 명시해 줄 수 있다.

```jsx
  jsx
  복사편집
  ReactDOM.render(
    <BrowserRouter basename="/레포이름">
      <App />
    </BrowserRouter>,
    document.getElementById('root')
  );
```





