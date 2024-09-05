# TIL - 2024-09-05

## 주제 : 스플랫 라우트

### 1. 개념 정의
- 스플랫 라우트는 URL의 일부가 고정되어 있고 나머지 부분이 동적으로 생성되는 라우트이다.

### 2. 주요 사용 사례 예시

#### **예시 1**: `/posts/design/figma` 에서 `/posts` 뒤의 모든 경로 읽기

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
└── /posts.$.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts.$.tsx`: `/posts/design/figma` 라우트를 처리, `loader`함수에서 `['*']`로 경로 출력함.

<br/>

Remix 코드 예시:
```tsx
// /posts.$.tsx (/posts/design/figma)

import { useLoaderData } from "@remix-run/react";
import { LoaderFunctionArgs } from "@remix-run/node";

export async function loader({ params }: LoaderFunctionArgs) {
  const param = params['*'];
  return param ; // design/figma
}

export default function Posts() {
  const param = useLoaderData<string>();
  return <div>Posts {param}</div>;
}
```

### 3. 주의할 점 / 참고할 점
- URL에 아무것도 없으면 기본적으로 비어 있다.
- 매개변수에 따라 사용자가 보고 싶어하는 콘텐츠를 표시할 수 있다.
- 고정된 부분 이후의 모든 동적 구간을 가져온다.

### 4. 추가 참고 자료
- [Remix v2 Lecture](https://www.udemy.com/course/remix-js-course/?couponCode=OF83024D)