# Server vs Client

Next.js에서는 서버 컴포넌트와 클라이언트 컴포넌트를 구분해 코드 일부가 서버 혹은 클라이언트에서 출력될 수 있도록 만들 수 있다.

기본적으로 생성하는 모든 컴포넌트는 서버 컴포넌트다.

클라이언트 컴포넌트로 변경/사용하려면 다음과 같이 컴포넌트 최상단에 ‘use client’ 선언이 되어 있어야 하고, 해당 선언이 없다면 서버 컴포넌트다.

다만 클라이언트 컴포넌트 또한 일부 정적인 요소들은 서버에서 렌더링하기 때문에, 클라이언트 컴포넌트는 ‘서버+클라이언트’의 하이브리드 컴포넌트라고 볼 수 있다.

서버/클라이언트 컴포넌트는 사용할 수 있는 일부 API가 다르다.

- 서버
  - cookies
  - headers
  - redirect
  - generateMetadata
  - revalidatePath
- 클라이언트
  - useState
  - useEffect
  - onClick
  - onChange
  - useRouter
  - useParams
  - useSearchParams
  - useFormState
  - useOptimistic

따라서 서버 컴포넌트에서는 보안, 캐싱, 성능, SEO 등의 이점이 있고, 클라이언트 컴포넌트에서는 상호작용(click, change) 브라우저 API(window, document) 활용 등의 이점을 가질 수 있다.

# 라우팅

# 파일 규칙

layout에서부터 순서대로 계층적인 구조를 나타내며, 각 페이지를 출력하기 위해 기능에 맞게 사용한다. 이를 명시적 컴포넌트 계층 구조라고 한다.

![image.png](attachment:8a84b3af-c720-4bfb-bba5-6e8fb956892c:image.png)

- layout: 고정 레이아웃
- template: 변화 레이아웃(탐색 과정)
- error: 에러 페이지
- loading: 로딩 페이지
- not-found: 찾을 수 없는 페이지(404)
- page: 기본 페이지

# 페이지

Next.js는 **폴더를 사용해 경로를 정의하는 파일 시스템 기반 라우터 방식**을 사용하므로, /app 폴더 내에 생성하는 각 폴더는 기본적으로 URL 경로를 의미한다.

만약 프로젝트에 /app/movies 폴더를 사용하면 로컬 기준 http://localhost:3000/movies 로 접근이 가능하게 된다. 여기서 출력되는 내용은 각 폴더의 page.tsx 컴포넌트 내용이다.

```json
// 프로젝트 구조 예시입니다.

├─app/
│  ├─movies/
│  │  └─page.tsx // localhost:3000/movies 의 페이지 내용
│  └─page.tsx // localhost:3000/ 의 페이지 내용(Home을 의미하게 되는 겁니다.)
```

파일 규칙에 해당하는 이름이 아닌 파일은 경로 자체로 정의되지 않기 때문에, 같은 폴더에서 여러 개의 파일을 추가하여 사용할 수는 있다.

예를 들어, app/movies 안에 들어 있는 page.tsx 파일은 경로로 인식되지만,

app/movies 안에 들어 있는 nav.tsx 파일은 경로로 인식되지 않는다.

# 레이아웃

여러 하위 경로에서 공통으로 사용되는 UI는 각 라우팅 폴더의 layout.tsx 컴포넌트에 작성한다. 슬롯 방식으로 children Prop을 사용하며, {children} 부분에는 같은 레벨에 있는 page.tsx에 들은 컴포넌트를 출력한다. 레이아웃은 중첩해서 사용할 수 있다.

```json
// 프로젝트 구조 예시입니다.

├─app/
│  ├─movies/
│  │  ├─layout.tsx
│  │  └─page.tsx
│  ├─layout.tsx
│  └─page.tsx
├─components/
│  └─Header.tsx
```

```tsx
// 코드 예시입니다.
// app/layout.tsx

import "./globals.css";
import Header from "@/components/Header";

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="ko">
      <body className="antialiased">
        <Header />
        <main className="p-2">{children}</main> // 여기에 app/page.tsx 출력
      </body>
    </html>
  );
}
```

# 페이지 이동 - 컴포넌트 방식의 탐색

Next.js에서는 페이지 이동을 위해 리액트와 비슷하게 Link 컴포넌트를 사용한다. 이동하는 페이지 전체를 새로고침하지 않고, 최적화된 번들만 일부 로드하거나 서버 렌더링 가능 등의 Next.js 프로젝트 내에서 최적화된 페이지 탐색을 제공한다.

```json
// 프로젝트 구조 예시입니다.
// components/Header.tsx에서 각 페이지로 이동할 수 있는 링크를 추가합니다.

├─app/
│  ├─movies/
│  │  ├─layout.tsx
│  │  └─page.tsx
│  ├─layout.tsx
│  └─page.tsx
├─components/
│  └─Header.tsx
```

```tsx
// 코드 예시입니다.
// /components/Header.tsx

import Link from "next/link";

export default function Header() {
  return (
    <header>
      <nav className="flex">
        {links.map(({ href, label }) => (
          <Link key={href} href={href} className="px-2">
            {label}
          </Link>
        ))}
      </nav>
    </header>
  );
}
```

# 동적 경로

미리 정의할 수 없는 동적 경로를 []를 사용하여 폴더 이름을 작성한다.

url의 세그먼트 값이 params의 Prop으로 전달되고, 대괄호 사이의 폴더 이름이 속성 이름이 된다.

쿼리스트링의 경우는, searchParams Prop으로 전달된다.

```json
// 프로젝트 구조 예시입니다.

├─app/
│  ├─movies/
│  │  ├─[movieId]/
│  │  │  └─page.tsx
```

```tsx
// 코드 예시입니다.
// /app/movies/[movieId]/page.tsx

interface Movie {
  Title: string;
  Plot: string;
}

export default async function MovieDetails({
  params, // 동적 세그먼트
  searchParams, // 쿼리스트링
}: {
  params: { movieId: string };
  searchParams: { plot?: "short" | "full" };
}) {
  const { movieId } = await params;
  const { plot } = await searchParams;
  const res = await fetch(
    `https://omdbapi.com/?apikey=7035c60c&i=${movieId}&plot=${plot || "short"}`
  );
  const movie: Movie = await res.json();
  return (
    <>
      <h1>{movie.Title}</h1>
      <p>{movie.Plot}</p>
    </>
  );
}
```
