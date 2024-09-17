# TIL - 2024-09-09

## 주제 : `loader` 함수

### 1. 개념 정의
- 각 라우트에서 데이터를 불러올 때 사용된다.
- 이 함수는 서버에서만 실행되며, 페이지가 처음 렌더링될 때나 브라우저에서 페이지 이동이 있을 때 데이터를 제공한다.

### 2. 주요 사용 사례 예시

#### **예시 1**: 데이터 불러오기

```ts
import { json } from "@remix-run/node"; // Remix의 json 유틸리티

export const loader = async () => {
  return json({ ok: true });
};
```
- 위 코드는 `loader` 함수는 서버에서 실행되며 `{ ok: true }`라는 데이터를 JSON 형식으로 반환한다.
- 서버에서만 접근 가능한 데이터나 API 호출을 안전하게 수행할 수 있다.

```ts
import { useLoaderData } from "@remix-run/react";

export default function Users() {
  const data = useLoaderData<typeof loader>();
  return (
    <ul>
      {data.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```
- `useLoaderData` 훅은 `loader` 함수가 반환한 데이터를 가져와서 컴포넌트 내에서 사용할 수 있도록 한다.
- `typeof loader`를 사용하면 타입 안전성을 유지할 수 있다.