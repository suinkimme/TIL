# TIL - 2024-09-05

## 주제 : 레이아웃 공유 없이 중첩 라우트 사용하기

### 1. 개념 정의
- 각 중첩된 경로가 독자덕인 레이아웃을 가지도록 설정하는 것이다.
- 기본적으로 Remix에서는 중첩된 경로가 있는 경우 상위 경로의 레이아웃을 하위 경로에서 공유한다. 그러나, 중첩된 경로들 간에 레이아웃을 공유하지 않도록 할 수 있다.

### 2. 주요 사용 사례 예시

#### **예시 1**: `/posts`, `/posts/design`, `/posts/design/figma` 중첩된 경로에서 레이아웃 공유 없이 라우팅

Remix에서 다음과 같은 구조의 파일이 있다고 가정해보자:

```bash
├── /posts.tsx                # /posts
├── /posts._index.tsx         # /posts
├── /posts.design.tsx         # /posts/design
├── /posts.design._index.tsx  # /posts/design
└── /posts.design.figma.tsx   # /posts/design/figma
```

이 구조는 다음과 같이 라우트를 표현한다.

1. `/posts.tsx`: `/posts` 라우트를 처리, `/posts._index.tsx` 가 하위에 렌더링 됨.
2. `/posts.design.tsx`: `/posts/design` 라우트를 처리, `/posts.design._index.tsx` 가 하위에 렌더링 됨, 상위 레이아웃 공유 없음.
3. `/posts.design.figma.tsx`: `/posts/design/figma` 라우트를 처리, 상위 레이아웃 공유 없음.

<br/>

### 3. 주의할 점 / 참고할 점
- `posts._index.tsx`가 생성되어 있으면 `posts.tsx`를 삭제해도 문제 없는 것을 확인함.

### 4. 추가 참고 자료
- [Remix v2 Lecture](https://www.udemy.com/course/remix-js-course/?couponCode=OF83024D)