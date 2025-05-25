# 📚 Frontend 실전 라이브러리 핵심 요약

## 1. Zustand

📌 **Lightweight 상태관리 라이브러리**

- Redux보다 훨씬 간단한 사용법 제공
- Hook 기반 API (`useStore`) 사용
- **불변성 관리 없이도 안전한 상태관리 가능**

### 기본 사용법

```ts
import { create } from "zustand";

const useStore = create((set, get) => ({
  count: 0,
  user: null,
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
  // 다른 상태값 참조하기
  doubleCount: () => get().count * 2,
}));

// 컴포넌트에서 사용
const { count, increase } = useStore();
```

### 실무 팁

- **Immer와 함께 사용**: 복잡한 중첩 객체 업데이트 시

```ts
import { immer } from "zustand/middleware/immer";

const useStore = create(
  immer((set) => ({
    user: { profile: { name: "", age: 0 } },
    updateName: (name) =>
      set((state) => {
        state.user.profile.name = name; // 불변성 걱정 없이 직접 수정
      }),
  }))
);
```

- **DevTools 연동**: 디버깅을 위한 Redux DevTools 사용

```ts
import { devtools } from "zustand/middleware";

const useStore = create(
  devtools(
    (set) => ({
      // 상태 정의
    }),
    { name: "my-store" }
  )
);
```

🔗 https://docs.pmnd.rs/zustand/getting-started/introduction

---

## 2. React Query (TanStack Query)

📌 **서버 상태 관리 라이브러리** (로컬이 아닌 원격 API 데이터 캐싱/제어)

- **자동 리패칭, 캐싱, 에러/로딩 관리 제공**
- 상태관리와는 다른 '데이터 패칭 도구'

### 기본 사용법

```ts
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// 데이터 조회
const { data, isLoading, error, refetch } = useQuery({
  queryKey: ["weather", location],
  queryFn: () => fetchWeather(location),
  staleTime: 5 * 60 * 1000, // 5분간 캐시 유지
  cacheTime: 10 * 60 * 1000, // 10분간 메모리에 보관
});

// 데이터 변경
const mutation = useMutation({
  mutationFn: updateWeather,
  onSuccess: () => {
    queryClient.invalidateQueries(["weather"]);
  },
});
```

### 실무 핵심 패턴

- **Optimistic Updates**: UI 먼저 업데이트 후 서버 동기화

```ts
const mutation = useMutation({
  mutationFn: updateTodo,
  onMutate: async (newTodo) => {
    await queryClient.cancelQueries(["todos"]);
    const previousTodos = queryClient.getQueryData(["todos"]);
    queryClient.setQueryData(["todos"], (old) => [...old, newTodo]);
    return { previousTodos };
  },
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(["todos"], context.previousTodos);
  },
});
```

- **Infinite Queries**: 무한 스크롤 구현

```ts
const { data, fetchNextPage, hasNextPage, isFetchingNextPage } =
  useInfiniteQuery({
    queryKey: ["posts"],
    queryFn: ({ pageParam = 0 }) => fetchPosts(pageParam),
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  });
```

🔗 https://tanstack.com/query/latest

---

## 3. React Hook Form + Zod

### 📌 React Hook Form

**폼 상태 제어 라이브러리**

- Uncontrolled 방식으로 퍼포먼스 우수
- 최소한의 리렌더링으로 빠른 성능

```ts
import { useForm } from "react-hook-form";

const {
  register,
  handleSubmit,
  watch,
  formState: { errors, isSubmitting },
} = useForm({
  defaultValues: { email: "", password: "" },
});

const onSubmit = (data) => console.log(data);

return (
  <form onSubmit={handleSubmit(onSubmit)}>
    <input {...register("email", { required: "이메일은 필수입니다" })} />
    {errors.email && <span>{errors.email.message}</span>}
  </form>
);
```

### 📌 Zod

**타입 안전한 스키마 기반 유효성 검사 라이브러리**

- TypeScript와 궁합 최고
- 런타임 타입 검증 + 컴파일 타임 타입 안전성

```ts
import { z } from "zod";
import { zodResolver } from "@hookform/resolvers/zod";

const schema = z
  .object({
    email: z.string().email("올바른 이메일을 입력하세요"),
    password: z.string().min(8, "비밀번호는 8자 이상이어야 합니다"),
    confirmPassword: z.string(),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: "비밀번호가 일치하지 않습니다",
    path: ["confirmPassword"],
  });

type FormData = z.infer<typeof schema>;

const { register, handleSubmit } = useForm<FormData>({
  resolver: zodResolver(schema),
});
```

### 실무 활용 패턴

- **동적 필드 관리**: `useFieldArray` 사용

```ts
const { fields, append, remove } = useFieldArray({
  control,
  name: "skills",
});

// 스킬 추가/삭제 UI 구현
```

🔗 React Hook Form: https://react-hook-form.com/
🔗 Zod: https://zod.dev/

---

## 4. Framer Motion

📌 **React 기반 애니메이션 라이브러리**

- 드래그, 전환, 상태 변화 애니메이션 구현 쉬움
- 선언적 API로 복잡한 애니메이션도 직관적

### 기본 애니메이션

```tsx
import { motion } from "framer-motion";

<motion.div
  initial={{ opacity: 0, y: 50 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0, y: -50 }}
  transition={{ duration: 0.5, ease: "easeOut" }}
>
  Hello World
</motion.div>;
```

### 실무 핵심 패턴

- **Layout Animations**: 자동 레이아웃 전환

```tsx
<motion.div layout>
  {items.map((item) => (
    <motion.div key={item.id} layout>
      {item.content}
    </motion.div>
  ))}
</motion.div>
```

- **Gesture 애니메이션**: 드래그, 호버, 탭

```tsx
<motion.div
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  drag="x"
  dragConstraints={{ left: -100, right: 100 }}
/>
```

- **스크롤 기반 애니메이션**

```tsx
import { useScroll, useTransform } from "framer-motion";

const { scrollYProgress } = useScroll();
const opacity = useTransform(scrollYProgress, [0, 1], [1, 0]);

<motion.div style={{ opacity }} />;
```

🔗 https://www.framer.com/motion/

---

## 5. Recharts

📌 **React 전용 차트 라이브러리** (라인, 바, 파이 등 지원)

- 컴포넌트 기반 구조로 React와 자연스럽게 통합
- 반응형 디자인 기본 지원

### 기본 사용법

```tsx
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
} from "recharts";

const data = [
  { name: "Jan", sales: 4000, profit: 2400 },
  { name: "Feb", sales: 3000, profit: 1398 },
  // ...
];

<ResponsiveContainer width="100%" height={400}>
  <LineChart data={data}>
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis dataKey="name" />
    <YAxis />
    <Tooltip />
    <Legend />
    <Line type="monotone" dataKey="sales" stroke="#8884d8" strokeWidth={2} />
    <Line type="monotone" dataKey="profit" stroke="#82ca9d" />
  </LineChart>
</ResponsiveContainer>;
```

### 실무 �팁

- **커스텀 툴팁**: 더 나은 UX를 위한 정보 표시

```tsx
const CustomTooltip = ({ active, payload, label }) => {
  if (active && payload && payload.length) {
    return (
      <div className="bg-white p-4 border rounded shadow">
        <p>{`시간: ${label}`}</p>
        <p style={{ color: payload[0].color }}>{`값: ${payload[0].value}`}</p>
      </div>
    );
  }
  return null;
};

<Tooltip content={<CustomTooltip />} />;
```

🔗 https://recharts.org/

---

## 6. Nivo

📌 **D3.js 기반 고급 React 차트 라이브러리**

- 다양한 테마, 커스터마이징, 애니메이션 지원
- 예쁘고 감각적인 시각화 구현에 적합

### 고급 시각화 예제

```tsx
import { ResponsiveLine } from "@nivo/line";

<ResponsiveLine
  data={data}
  margin={{ top: 50, right: 110, bottom: 50, left: 60 }}
  xScale={{ type: "point" }}
  yScale={{
    type: "linear",
    min: "auto",
    max: "auto",
    stacked: false,
    reverse: false,
  }}
  curve="cardinal"
  axisTop={null}
  axisRight={null}
  axisBottom={{
    orient: "bottom",
    tickSize: 5,
    tickPadding: 5,
    tickRotation: 0,
  }}
  colors={{ scheme: "nivo" }}
  pointSize={10}
  pointColor={{ theme: "background" }}
  pointBorderWidth={2}
  pointBorderColor={{ from: "serieColor" }}
  useMesh={true}
  animate={true}
  motionStiffness={90}
  motionDamping={15}
/>;
```

### 특화 차트들

- **히트맵**: `ResponsiveHeatMap`
- **트리맵**: `ResponsiveTreeMap`
- **선버스트**: `ResponsiveSunburst`
- **스트림**: `ResponsiveStream`

🔗 https://nivo.rocks/

---

## 7. Victory

📌 **React 기반 시각화 라이브러리**

- 애니메이션 내장, 정적인 보고서 UI에 적합
- 모듈식 구조로 필요한 부분만 import 가능

```tsx
import {
  VictoryChart,
  VictoryLine,
  VictoryAxis,
  VictoryTooltip,
} from "victory";

<VictoryChart
  theme={VictoryTheme.material}
  height={400}
  width={600}
  padding={{ left: 80, top: 40, right: 80, bottom: 40 }}
>
  <VictoryAxis />
  <VictoryAxis dependentAxis />
  <VictoryLine
    data={data}
    x="x"
    y="y"
    style={{
      data: { stroke: "#c43a31" },
      parent: { border: "1px solid #ccc" },
    }}
    animate={{
      duration: 2000,
      onLoad: { duration: 1000 },
    }}
    labelComponent={<VictoryTooltip />}
  />
</VictoryChart>;
```

🔗 https://formidable.com/open-source/victory/

---

## ✅ 실무 추천 조합 & 선택 기준

| 용도               | 1순위                 | 2순위           | 선택 기준                          |
| ------------------ | --------------------- | --------------- | ---------------------------------- |
| **전역 상태관리**  | Zustand               | Context API     | 팀 규모, 복잡도에 따라             |
| **서버 상태 캐싱** | React Query           | SWR             | React Query가 더 풍부한 기능       |
| **폼 처리**        | React Hook Form + Zod | Formik + Yup    | 성능과 타입 안전성 우선            |
| **간단한 차트**    | Recharts              | Chart.js        | React 전용 vs 범용                 |
| **고급 시각화**    | Nivo                  | D3.js           | 개발 편의성 vs 완전한 커스터마이징 |
| **애니메이션**     | Framer Motion         | CSS Transitions | 복잡도와 성능 요구사항             |

### 프로젝트별 스택 조합 예시

**📊 대시보드 프로젝트**

```
- Zustand (전역 상태)
- React Query (API 데이터)
- Recharts (기본 차트)
- Framer Motion (페이지 전환)
```

**📝 관리자 페이지**

```
- Context API (간단한 상태)
- React Query (CRUD 작업)
- React Hook Form + Zod (복잡한 폼)
- 차트 라이브러리 없음
```

**🎨 포트폴리오/마케팅 사이트**

```
- 전역 상태 없음
- Nivo (인상적인 시각화)
- Framer Motion (풍부한 애니메이션)
```

### 성능 고려사항

- **번들 사이즈**: Recharts < Victory < Nivo
- **런타임 성능**: Zustand > Redux, React Hook Form > Formik
- **학습 곡선**: Zustand < Redux, Recharts < D3.js

---

## 🔧 설치 및 초기 설정

### 필수 의존성

```bash
# 상태관리
npm install zustand
npm install @tanstack/react-query

# 폼 처리
npm install react-hook-form zod @hookform/resolvers

# 시각화 (택 1)
npm install recharts
npm install @nivo/core @nivo/line @nivo/bar # 필요한 차트만
npm install victory

# 애니메이션
npm install framer-motion
```

### React Query 초기 설정

```tsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5분
      cacheTime: 10 * 60 * 1000, // 10분
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```
