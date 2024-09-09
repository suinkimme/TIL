# TIL - 2024-09-06

## 주제 : Remix에서 메타 태그 추가하기

### 1. 개념 정의
- Remix의 `meta` 함수는 각 경로(route)에 대해 HTML 메타 태그를 추가할 수 있게 해준다.
- 메타 태그 용도
  - SEO(검색 엔진 최적화)
  - 브라우저 동작 제어
  - 소셜 미디어에서의 풍부한 미리보기 `(og)`

### 2. 주요 사용 사례 예시

#### **예시 1**: 기본 메타데이터 설정

```ts
export const meta: MetaFunction = () => {
  return [
    { title: "아주 멋진 앱 | Remix" },
    { property: "og:title", content: "아주 멋진 앱" },
    { name: "description", content: "이 앱은 최고입니다" },
  ];
};
```
이 코드는 다음과 같은 HTML 태그를 생성함:
```html
<title>아주 멋진 앱 | Remix</title>
<meta property="og:title" content="아주 멋진 앱" />
<meta name="description" content="이 앱은 최고입니다" />
```

#### **예시 2**: 특수한 메타 태그 사용

```ts
export const meta: MetaFunction = () => {
  return [
    {
      "script:ld+json": {
        "@context": "https://schema.org",
        "@type": "Organization",
        name: "Remix",
        url: "https://remix.run",
      },
    },
  ];
};
```
이 코드는 다음과 같은 HTML을 생섬함:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Remix",
  "url": "https://remix.run"
}
</script>
```
- `title`: 페이지 제목을 설정함. 이는 `<title>` 태그로 렌덩링됨.
- `url`: 특정 엔티티(예: 조직, 제품, 이벤트 등)에 대한 공식 웹 페이지나 리소스를 명시함.
- `script:ld+json`: 구조화된 데이터(JSON0LD)를 제공하여, `<script type="application/ld+json">` 태그로 렌더링됨.
  - 이 JSON-LD 스크립트는 검색 엔진이 페이지의 컨텐츠를 더 잘 이해하도록 도와주는 정보다. 페이지가 어떤 조직에 관한 것인지 명확하게 알 수 있다.

#### **예시 3**: `<link>` 태그 생성:

```ts
export const meta: MetaFunction = () => {
  return [
    {
      tagName: "link",
      rel: "canonical",
      href: "https://remix.run",
    },
  ];
};
```
이 코드는 다음과 같은 HTML을 생섬함:
```html
<link rel="canonical" href="https://remix.run" />
```
- `tagName`: 속성을 `link`로 설정하면 `<link>` 태그를 생성할 수 있음.

#### **예시 4**: `meta` 함수에서 매개변수 활용하기

### `location`

```ts
export const meta: MetaFunction = ({ location }) => {
  const searchQuery = new URLSearchParams(location.search).get("q");
  return [{ title: `"${searchQuery}"에 대한 검색 결과` }];
};
```
- 현재 라우터의 위치 객체를 제공함. 특정 경로 또는 쿼리 파라미터에 따라 메타 태그를 생성하는 데 유용함.

### `matches`

```ts
export const meta: MetaFunction = ({ matches }) => {
  const parentMeta = matches.flatMap((match) => match.meta ?? []);
  return [...parentMeta, { title: "프로젝트" }];
};
```
- 현재 경로의 일치 항목 배열로, 부모 라우트의 메타 데이터를 포함 하여 여러 메타 정보를 병합하는 데 사용할 수 있음.
  - 자식 라우트에서 `meta` 함수를 정의할 수 있지만, 가장 마지막에 일치한 라우트의 `meta` 함수만 최종적으로 적용됨. _(그래서 이렇게 병합 기능을 제공함)_

### `data`
```ts
export const meta: MetaFunction<typeof loader> = ({ data }) => {
  return [{ title: data.task.name }];
};
```
- 해당 경로의 `loader`에서 반환된 데이터를 제공함. 이 테이터를 사용해 동적으로 메타 데이터를 설정할 수 있음.

### `params`
- URL의 동적 세그먼트를 제공함.

### `error`
```ts
export const meta: MetaFunction = ({ error }) => {
  return [{ title: error ? "oops!" : "Actual title" }];
};
```
- 해당 경로에서 발생한 에러 정보를 담고 있음.


### 3. 주의할 점 / 참고할 점
-

### 4. 추가 참고 자료
- [Remix docs - meta](https://remix.run/docs/en/main/route/meta)