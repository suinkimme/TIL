# TIL - 2024-09-05

## 주제 : `<Link />`로 클라이언트 네비게이션 구현하기

### 1. 개념 정의
- `<a href>` 태그를 클라이언트 측 라우팅과 함께 사용할 수 있도록 해주는 **Wrapper Component** 이다.
  - `<Link />` 컴포넌트는 React Router나 Remix에서 `<a>` 태그를 대체하여 사용된다.

### 2. 주요 사용 사례 예시

#### **예시 1**: Props 살펴보기

### `to: string`:

```tsx
<Link to="/some/path">
```

- `to` 속성은 `<Link />` 컴포넌트에서 필수적으로 사용된다.
  - 사용자가 클릭 했을 때 이동할 경로를 지정함.
- 클라이언트 측 라우팅을 지원하여 페이지 전체 새로 고침 없이 브라우저의 URL을 변경하고 필요한 데이터를 로드함.

<br/>

### `to: Partial<path>`:

```tsx
<Link
  to={{
    pathname: "/some/path",
    search: "?query=string",
    hash: "#hash",
  }}
/>
```

- `Partial<path>` 형태의 값을 전달하면, 이동할 경로에 대한 세부적인 정보를 객체 형태로 지정할 수 있다.
  - `pathname`: 이동할 URL 경로
  - `search`: URL의 쿼리 문자열로, `?`로 시작된다.
  - `hash`: URL의 해시(fragment) 부분으로, `#`로 시작된다.

<br/>

### `discover`:

```tsx
<>
  <Link /> {/* defaults to "render" */}
  <Link discover="none" />
</>
```

- `future.unstable_lazyRouteDiscovery` 기능과 함꼐 사용될 때, 경로를 언제 탐색(discover)할지 설정하는 데 유용함.
  - `render` (기본값): 링크가 렌더링될 때 경로를 탐색함, 링크가 페이지에 나타나면 Remix는 해당 경로를 미리 탐색하여 필요한 정보를 준비함.
  - `none`: 링크를 미리 탐색하지 않고, 사용자가 링크를 클릭할 때에만 탐색함.

<br/>

### `prefetch`:

```tsx
<>
  <Link /> {/* defaults to "none" */}
  <Link prefetch="none" />
  <Link prefetch="intent" />
  <Link prefetch="render" />
  <Link prefetch="viewport" />
</>
```

- `prefetch` 속성은 Remix의 `<Link />` 컴포넌트에서 특정 경로에 대한 데이터와 모듈을 **미리 가져오는(pre-fetching)** 동작을 제어한다.
  - 사용자가 링크를 클릭하기 전에 해당 경로의 리소스를 미리 로드할 수 있음.
  - 클릭 시 즉각적인 응답을 제공하고 사용자 경험을 향샹시킬 수 있음.
- 옵션 및 동작
  - `none` (기본값): prefetch 동작을 수행하지 않음
  - `intent`: 사용자가 `<Link />`에 마우스를 올리거나(Tab/Focus 등) 초점을 맞출 때 데이터를 미리 가져옴.
  - `render`: `<Link />`가 렌더링될 때 데이터를 미리 가져옴.
    - 많은 링크에서 사용하면 성능에 영향을 줌 **_신중하게 사용할 것_**
  - `viewport`: `<Link />`가 뷰포트(viewport)에 들어올 때 데이터를 미리 가져옴.

```tsx
<nav>
  <a href="/about">About Us</a>
  <a href="/contact">Contact</a>
  <Link rel="prefetch" href="/about" /> // prefetch가 활성화된 경우 추가됨
</nav>
```

주의 사항: CSS 선택자와의 관계
- `prefetch` 속성으로 인해 `<Link rel="prefetch">` 태그가 동적으로 추가될 수 있으므로, CSS 선택자 사용에 주의해야 한다.
  - `<nav>` 내부의 마지막 자식 요소 `<a>`를 선택할 때 `nav :last-child`가 아닌 `nav a:last-of-type`을 사용해야 한다.

<br/>

### `preventScrollReset`:

```tsx
<Link to="?tab=one" preventScrollReset />
```

- `<Link />`를 클릭해 새로운 페이지나 경로로 이동할 때 스크롤 위치가 초기화되는 것을 방지함.
  - `<ScrollRestoration />` 컴포넌트와 함께 사용할 때 유용하다고 함. **_아직 정확한 동작 방식을 확인하지 못함_**
- 이 속성은 브라우저의 "뒤로" 또는 "앞으로" 버튼을 클릭할 때의 스크롤 위치 복원 동작에는 영향을 미치지 않음.
  - `<Link />`를 클릭할 때의 동작에만 영향을 줌.

<br/>

### `relative`:

```tsx
<Link to=".." /> // default: "route"
<Link relative="route" />
<Link relative="path" />
```

- `<Link />` 컴포넌트에서 상대 경로의 해석 방식을 정의함.
  - 현재 위치에서 상대적으로 어떻게 해석될지를 지정할 수 있음.
  - 옵션
    - `route` (기본값): 현재 라우트를 기준으로 상대 경로를 해석함. 동적 라우트를 포함한 모든 URL 매개변수가 무시됨.
    - `path`: 현재 URL 경로를 기준으로 이동하며, 동적 라우트가 무시되지 않음.

<br/>

### `reloadDocument`:

```tsx
<Link to="/logout" reloadDocument />
```

- 클라이언트 사이드 라우팅 대신 브라우저의 기본 문서 탐색 동작을 사용하도록 설정함.
  - `<a href>` 태그와 동일한 방식으로 브라우저가 페이지를 새로 고침하고 이동함.
- 로그아웃과 같은 서버 측 상태 변경 시, SEO나 접근성, 특정 외부 리소스 로드가 필요한 경우에 유용하게 사용됨.

<br/>

### `replace`:

```tsx
<Link replace />
```

- **브라우저 히스토리 스택(history stack)** 을 제어하는 기능
  - 현재 히스토리 항목을 대체함.
- 동작 예시
  - 현재 히스토리가 `A -> B`인 상태에서 링크를 클릭하면, 히스토리 스택은 `A -> C`가 된다.

<br/>

### `state`:

```tsx
<Link to="/somewhere/else" state={{ some: "value" }} />
```
```tsx
function SomeComp() {
  const location = useLocation();
  location.state; // { some: "value" }
}
```

- 링크를 클릭해 다른 페이지로 이동할 때, 특정 상태 데이터를 함께 전달하고 싶을 때 `state` 속성을 사용함.
  - **브라우저의 `history.state`** 를 기반으로 하여, 서버에서는 접근할 수 없다.

<br/>

### `unstable_viewTransition`: ~~업데이트 없이 사라질 수 있음~~

```tsx
<Link to={to} unstable_viewTransition>
  Click me
</Link>
```

- Remix에서 제공하는 **실험적 기능**으로 View Transition API를 활용하여 네비게이션 시 부드러운 전환 애니메이션을 구현할 수 있게 함.

예제 코드: 
```tsx
function ImageLink(to) {
  const isTransitioning =
    unstable_useViewTransitionState(to);
  return (
    <Link to={to} unstable_viewTransition>
      {/* View Transition 이름을 설정하여 CSS 애니메이션 적용 */}
      <p
        style={{
          viewTransitionName: isTransitioning
            ? "image-title"
            : "",
        }}
      >
        Image Number {idx}
      </p>
      <img
        src={src}
        alt={`Img ${idx}`}
        style={{
          viewTransitionName: isTransitioning
            ? "image-expand"
            : "",
        }}
      />
    </Link>
  );
}
```

### 3. 주의할 점 / 참고할 점
- `future.unstable_lazyRouteDiscovery`: Remix는 일반적으로 애플리케이션의 모든 경로를 미리 탐색하고, 사용자가 해당 경로로 이동할 때 필요한 모든 데이터를 준비한다. 이것을 활성화하면 모든 경로를 즉시 탐색하지 않고, 사용자가 경로에 접근할 때만 탐색을 수행한다. **_(이 기능은 현재 불안정 상태로, 실험적으로 제공됨)_**

### 4. 추가 참고 자료
- [Remix docs - Link](https://remix.run/docs/en/main/components/link#discover)