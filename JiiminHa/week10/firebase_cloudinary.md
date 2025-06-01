# Firebase & Cloudinary 통합 정리 (React 기반)

## 1. Firebase

### 1-1. Firebase란?

Firebase는 Google에서 제공하는 모바일/웹 앱 개발 플랫폼. 서버리스 환경에서 인증, 데이터베이스, 저장소, 호스팅 등 다양한 백엔드 기능을 제공.

### 1-2. 주요 기능

| 기능명                   | 설명                                               |
| ------------------------ | -------------------------------------------------- |
| Authentication           | 로그인/회원가입 구현 (이메일, Google, Github 등)   |
| Firestore                | 실시간 데이터 동기화가 가능한 NoSQL DB             |
| Cloud Storage            | 이미지, 동영상 등 파일 업로드용 저장소             |
| Hosting                  | 정적 웹 앱을 빠르게 배포 가능                      |
| Cloud Functions          | 서버 없이 백엔드 로직을 실행할 수 있는 함수 서비스 |
| Messaging / Analytics 등 | 푸시 알림, 사용자 분석 기능                        |

---

### 1-3. Firebase 프로젝트 셋업 (React)

#### 1) Firebase 콘솔 설정

- [Firebase Console](https://console.firebase.google.com/) 접속
- 새 프로젝트 생성
- "웹 앱 등록" 클릭 → Firebase SDK config 정보 복사
- Authentication, Firestore, Storage 등 필요한 기능 활성화

#### 2) 패키지 설치

```bash
npm install firebase
```

#### 3) 초기화 코드 (firebase.js)

```js
// src/firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";
import { getStorage } from "firebase/storage";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
};

const app = initializeApp(firebaseConfig);

export const auth = getAuth(app);
export const db = getFirestore(app);
export const storage = getStorage(app);
```

---

## 2. Cloudinary

### 2-1. Cloudinary란?

Cloudinary는 이미지 및 영상의 업로드, 저장, 최적화, 전송까지 지원하는 클라우드 기반 미디어 관리 서비스.

### 2-2. 주요 기능

| 기능명        | 설명                                                    |
| ------------- | ------------------------------------------------------- |
| 업로드        | 사용자 이미지/영상 파일을 서버 없이 업로드              |
| 변환          | 크기 조절, 포맷 변경, 자동 최적화 (URL 파라미터로 조작) |
| CDN 전달      | 글로벌 CDN을 통해 빠르게 콘텐츠 전송                    |
| AI 기능       | 얼굴 중심 크롭, 배경 제거, 자동 품질 조정 등            |
| Upload Widget | 프론트에서 쉽게 업로드할 수 있는 UI 제공                |

---

### 2-3. Cloudinary 셋업 및 사용법 (React)

#### 1) Cloudinary 계정 생성 및 설정

- [Cloudinary](https://cloudinary.com/) 가입
- Dashboard에서 `Cloud name`, `Upload Preset` 확인
- Upload Preset은 `unsigned`로 생성 (서버 없는 프론트 전용)

#### 2) 스크립트 추가

`public/index.html` 또는 `_document.tsx`에 추가:

```html
<script
  src="https://upload-widget.cloudinary.com/global/all.js"
  type="text/javascript"
></script>
```

#### 3) 업로드 위젯 코드

```js
// 사용자가 버튼 클릭 시 호출
const openCloudinaryWidget = () => {
  window.cloudinary.openUploadWidget(
    {
      cloudName: "YOUR_CLOUD_NAME",
      uploadPreset: "YOUR_UPLOAD_PRESET",
      sources: ["local", "camera", "url"],
      multiple: false,
    },
    (error, result) => {
      if (!error && result && result.event === "success") {
        console.log("Uploaded URL:", result.info.secure_url);
        // 이 URL을 Firebase DB나 상태에 저장 가능
      }
    }
  );
};
```

---

## 3. Firebase vs Cloudinary 비교 요약

| 항목        | Firebase                          | Cloudinary                      |
| ----------- | --------------------------------- | ------------------------------- |
| 주요 목적   | 전체 백엔드 (인증, DB, 저장소 등) | 이미지/영상 업로드 및 최적화    |
| 저장 대상   | 데이터 + 파일                     | 이미지/영상                     |
| DB 기능     | Firestore, Realtime DB 제공       | 없음                            |
| 이미지 변환 | 별도 구현 필요                    | URL 기반 자동 변환 지원         |
| CDN 기능    | 일부 존재 (Storage/Hosting)       | 글로벌 CDN 내장                 |
| 사용 난이도 | 다소 복잡 (기능 다양)             | 이미지 중심이라 상대적으로 단순 |

---

## 4. 참고 자료

- Firebase Docs: https://firebase.google.com/docs
- Cloudinary Docs: https://cloudinary.com/documentation
- Cloudinary Upload Widget: https://cloudinary.com/documentation/upload_widget
- Firebase with React 예제: https://github.com/firebase/quickstart-js
