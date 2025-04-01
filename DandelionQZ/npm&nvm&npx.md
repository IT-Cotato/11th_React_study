### npm

npm은 node package manager의 줄임말로 자바스크립트 패키지 매니저이다. Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(Command line Interface)를 제공한다. Node.js를 설치할 때 npm도 같이 설치된다. 

### Node.js

Node.js는 JavaScript 코드를 브라우저 밖에서 실행할 수 있게 해주는 런타임 환경이다. 크롬 V8 자바스크립트 엔진으로 빌드한 자바스크림트 런트임으로 웹 브라우저 환경이 아닌 곳에서도 자바스크립트를 사용하여 연산가능하다. 

### nvm

nvm은 Node Version Manager.의 줄임말로 Node.js의 버전을 관리하는 버전 관리자이다. 서로 다른 버전을 설치하고 삭제하는 과정이 번거로우니 미리 설치해두고 필요할 때마다 특정 버전을 활성화하는 프로그램이다. 

### npx

npx는 Node Package eXecute의 줄임말로 npm과 비교대상이 아니라 npm을 더 편하게 사용하기 위한 도구이다. npm 5.2.0 버전 이상부터 npm을 설치하면 자동으로 npx가 설치된다. 

사용하는 이유는 npm으로 패키지를 설치시 ‘전역으로 패키지를 설치하여 의존성 라이브러리들을 전체적으로 관리하는 방법’과 ‘특정 프로젝트에만 의존성 라이브러리를 설치하는 방법’이 있는데 이렇게 Dependency로 라이브러리가 관리되면 이후 업데이트시 관리가 쉽지 않다. 따라서 npx를 이용하면 직접 패키지 설치나 업데이트를 하지 않아도 npm 레지스트리에 올라가 있는 최신 버전을 실행 및 설치할 수 있어 간편하다. 

### 참고

https://20002100.tistory.com/entry/nvm%EA%B3%BC-npm-%EA%B7%B8%EB%A6%AC%EA%B3%A0-npx%EC%9D%98-%EC%B0%A8%EC%9D%B4

https://namu.wiki/w/Node.js?from=Nodejs#s-6.1

https://youngmin.hashnode.dev/npm-npx
