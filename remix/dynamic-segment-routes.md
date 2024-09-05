# TIL - 2024-09-05

## 주제 : 동적 라우트 적용, URL 매개변수 읽기

### 1. 개념 정의
- 파일 이름에 `$ooo` 기호를 사용하는 경우, 해당 부분이 동적으로 변할 수 있는 URL 세그먼트를 의미한다.

### 2. 주요 사용 사례 예시

#### **예시 1**: `/posts/hello-world` 페이지의 URL 매개변수 읽기

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
├── /posts.tsx
└── /posts.$slug.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts.tsx`: `/posts` 라우트를 처리
2. `/posts.$slug.tsx`: `/posts/hello-world` 로 접속할 경우 해당 라우트를 처리함.
3. `/posts/hello-world`: `$slug`는 `hello-world`로 인식됨.

<br/>

Remix 코드 예시:
```tsx
// posts.$slug.tsx

import { useLoaderData } from "@remix-run/react";
import { LoaderFunctionArgs } from "@remix-run/node";

export async function loader({ params }: LoaderFunctionArgs) {
  const slug = params.slug;
  return slug; // $slug = hello-world
}

export default function Posts() {
  const slug = useLoaderData<string>();
  return <div>Posts {slug}</div>;
}

```

### 3. 주의할 점 / 참고할 점
- `loader` 함수는 항상 데이터를 반환하거나, 최소한 `null`을 반환해야 함.
- Remix는 `lodaer` 함수가 데이터를 반환하여 클라이언트 컴포넌트에서 사용할 수 있도록 함.

### 4. 추가 참고 자료
- [Remix v2 Lecture](https://www.udemy.com/course/remix-js-course/?couponCode=OF83024D)