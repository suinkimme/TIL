# TIL - 2024-09-05

## 주제 : 선택적 라우트 적용, URL 매개변수 읽기

### 1. 개념 정의
- 파일 이름에 `($)` 기호를 사용하는 경우, 해당 부분이 선택적인 URL 세그먼트를 의미한다.

### 2. 주요 사용 사례 예시

#### **예시 1**: `/posts`, `/fr/posts` 페이지의 선택적인 URL 매개변수 읽기

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
└── /($lang).posts.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts`: `/posts` 라우트를 처리 `$lang` 매개변수는 `undefined`로 인식됨.
2. `/en/posts`: `/en/posts` 라우트를 처리,  `$lang` 매개변수는 `en`으로 인식됨.

<br/>

Remix 코드 예시:
```tsx
// ($lang).posts.tsx (/en/posts)

import { useLoaderData } from "@remix-run/react";
import { LoaderFunctionArgs } from "@remix-run/node";

export async function loader({ params }: LoaderFunctionArgs) {
  const slug = params.slug;
  return slug; // $lang = en or undefined
}

export default function Posts() {
  const slug = useLoaderData<string>();
  return <div>Posts {slug}</div>;
}

```

### 3. 주의할 점 / 참고할 점
- 매개변수가 없어 `undefined`로 인식되는 경우, `loader` 함수에서 예외 처리 하지 않으면 에러가 발생함.

### 4. 추가 참고 자료
- [Remix v2 Lecture](https://www.udemy.com/course/remix-js-course/?couponCode=OF83024D)