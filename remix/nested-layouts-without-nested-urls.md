# TIL - 2024-09-05

## 주제 : 동일한 레이아웃, 다른 URL 구조: 효율적인 페이지 구성

### 1. 개념 정의
- URL의 계층 구조를 유지하지 않고 다양한 레이아웃을 서로 다른 경로에 적용할 수 있다.
  - URL을 단순하고 깔끔하게 유지하면서도 중첩 레이아웃을 사용할 수 있다.

### 2. 주요 사용 사례 예시

#### **예시 1**: `/login`, `/register` 페이지의 레이아웃 공유

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
├── _auth.login.tsx     # /login
├── _auth.register.tsx  # /register
└── _auth.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/_auth.login.tsx`: `/login` 라우트를 처리
2. `/_auth.register.tsx`: `/register` 라우트를 처리
3. `/_auth.tsx`: 두 라우트의 공통 레이아웃을 처리

<br/>

예를 들어, 사용자가 `/login` 페이지를 볼 때, Remix는 아래 처럼 렌더링한다.

```tsx
<Auth>         // _auth.tsx
  <Login />    // _auth.login.tsx
</Auth>
```
<br/>

Remix 코드 예시:
```tsx
// _auth.tsx

import { Outlet } from "@remix-run/react";

export default function Auth() {
  return (
    <div>
      <h1>Auth Layout</h1>
      <Outlet />
    </div>
  );
}
```

### 3. 주의할 점 / 참고할 점
- `pathless routes` 라고 부르기도 한다고 함.

### 4. 추가 참고 자료
- [Remix v2 Lecture](https://www.udemy.com/course/remix-js-course/?couponCode=OF83024D)