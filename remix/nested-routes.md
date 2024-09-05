# TIL - 2024-09-04

## 주제 : 중첩 라우트

### 1. 개념 정의
- **중첩 라우트**는 말 그대로 여러 개의 라우트를 계층적으로 구성하는 것을 말한다.
  - 각 라우트마다 고유한 컴포넌트와 데이터를 로드할 수 있도록 설계되어 있다. 페이지의 일부만 바꿔야 할 때 전체 페이지를 렌더링하지 않고, 필요한 부분(해당 라우트)만 업데이트할 수 있다.

### 2. 예시

#### **예시 1**: `/posts/design/figma` 페이지

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
├── posts.tsx
├── posts.design.tsx
└── posts.design.figma.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts.tsx`: `/posts` 라우트를 처리
2. `/posts.design.tsx`: `/posts/design` 라우트를 처리
3. `/posts.design.figma.tsx`: `/posts/design/figma` 라우트를 처리

<br/>

예를 들어, 사용자가 `/posts/design/figma` 페이지를 볼 때, Remix는 이 세 개의 라우트를 모두 랜더링하게 된다.

```tsx
<Posts>         // posts.tsx
  <Design>      // posts.design.tsx
    <Figma />   // posts.design.figma.tsx
  </Design>
</Posts>
```
<br/>

Remix에서 중첩 라우트를 구현할 때.

```tsx
// posts.tsx

import { Outlet } from "@remix-run/react";

export default function Posts() {
  return (
    <div>
      <h1>Posts</h1>
      <Outlet />
    </div>
  );
}
```
> `/posts.design.tsx` 에도 동일하게 Outlet을 사용해야 함.

#### **예시 2**: 레이아웃 공유가 필요 없지만 중첩 라우트를 사용하고 싶은 경우

\[**예시 1**\]의 구조를 다음과 같이 변경해보자:

```bash
├── posts.tsx
├── posts.design_.tsx # (_) 추가
└── posts.design.figma.tsx
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts.tsx`: `/posts` 라우트를 처리
2. `/posts.design_.tsx`: `/posts/design` 라우트를 처리
3. `/posts.design.figma.tsx`: `/posts/design/figma` 라우트를 처리, 상위 라우트인 `/posts.design_.tsx` 라우트는 처리하지 않음.

### 3. 주의할 점 / 참고할 점
- 중첩 라우트를 사용하여 여러 라우트가 공통으로 사용하는 레이아웃을 공유하는 패턴을 **Layout Sharing**이라고 함.