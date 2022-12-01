# React Router의 Route

> 기존에 코드숨 과정을 진행하면서 복잡한 Route Config를 짤 일이 있었는데 중첩 `Route`를 공부하고 수정하면서 알게된 내용입니다.

<br />

segment가 몇 단계 더 들어가는 url 주소에 페이지를 만들어줘야하는 상황이었다.

## 1. 기존 방식

- 특정 경로 접근 시 해당 컴포넌트로 접근하게 해서 해당 컴포넌트에서 또 Routes를 작성해주는 방식으로 로직을 짰다.

```tsx
// App.tsx

<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="/counter" element={<CounterPage />} />
  <Route path="/todos" element={<TodosPage />} />
  <Route path="/register-restaurant" element={<RegisterRestaurantPage />} />
  <Route path="/restaurants-app/*" element={<RestaurantsApp />} />
  <Route path="*" element={<NotFoundPage />} />
</Routes>
```

```tsx
// RestaurantsApp.tsx

<>
  <Header />
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/about" element={<AboutPage />} />
    <Route path="/login" element={<LoginPage />} />
    <Route path="/restaurants" element={<RestaurantsPage />} />
    <Route
      path="/restaurants/:restaurantId"
      element={<RestaurantDetailPage />}
    />
  </Routes>
</>
```

<br />

## 2. 중첩 Route 활용

```tsx
// App.tsx

<Routes>
  <Route index element={<HomePage />} />
  <Route path="/counter" element={<CounterPage />} />
  <Route path="/todos" element={<TodosPage />} />
  <Route path="/register-restaurant" element={<RegisterRestaurantPage />} />
  <Route path="restaurants-app" element={<RestaurantsApp />}>
    <Route index element={<RestaurantsHomePage />} />
    <Route path="about" element={<AboutPage />} />
    <Route path="login" element={<LoginPage />} />
    <Route path="restaurants">
      <Route index element={<RestaurantsPage />} />
      <Route path=":restaurantId" element={<RestaurantDetailPage />} />
    </Route>
  </Route>
  <Route path="*" element={<NotFoundPage />} />
</Routes>
```

```tsx
// RestaurantsApp.tsx

import { Outlet } from "react-router-dom";

<>
  <Header />
  <Outlet />
</>;
```

<br />

## 3. 알게된 점

- index Route: 경로를 설정하지 않는 대신 기본 하위 경로로 자동 설정하는 속성
- 중첩 라우팅

  - 위의 경로에서 `/restaurants-app`에 접근하면 아래와 같이 렌더링된다.
    ```tsx
    <RestaurantsApp>
      <RestaurantsHomePage />
    </RestaurantsApp>
    ```
  - RestaurantsApp에서 중첩된 컴포넌트를 가지고 올 때는 `react`의 `children`을 사용하는 것이 아닌 `react-router-dom`에서 제공하는 `Outlet`을 사용한다.

  - 중첩 라우팅을 할 때 감싸는 Route의 경로를 설정 안 하므로써 Layout전용 Route를 만들수도 있고 element를 설정 안 하므로써 경로를 단순화하는 방식으로 사용할 수 있다.

- `BrowserRouter`는 History 객체를 생성, 초기 위치 상태 생성, URL을 참조 한다.
- `Routes`는 `Route Config`라는 경로 객체 트리를 생성한다.
- History 객체: History Stack을 조작할 수 있도록 API를 제공하는 객체 (접속 이력 스택으로 브라우저의 뒤로 가기 및 앞으로 가기 버튼을 클릭할 때마다 참조하는 스택)
- Route Config: 모든 `Route`에 대한 설정을 정리해놓은 경로 객체 트리 (위에서 `Routes`와 `Route`로 작성해놓은 코드)
- Location 객체: 브라우저의 빌트인 객체 `window.location`을 기반으로 하는 React Router에서 제공하는 객체, `useLocation` 훅으로 접근 가능

  ```javascript
  // Location 객체

  {
    pathname: "/bbq/pig-pickins";,
    search: "?campaign=instagram",
    hash: '#menu',
    state: null,
    key: "aefz24ie"
  }
  ```

  - state: 이전 페이지로부터 전달받은 값

    ```tsx
    <Link to="/pins/123" state={{ fromDashboard: true }} />;

    navigate("/users/123", { state: partialUser });
    ```

  - key: Location 객체의 고유값, 이를 잘 활용하면 뒤로 가기를 클릭했을 때 서버에 데이터를 요청하는 과정을 건너 뛸 수 있다.

- Index Routes
  - default child route로 동작
  - 부모 경로에서 + `/`인 경우 동작
  - 모든 레벨에서 사용 가능

<br />

## Reference

- [React Router 사용법 (v6)](https://charles098.tistory.com/182)
- [React Router v6 정리](https://velog.io/@tjdgus0528/React-Router-v6-%EC%A0%95%EB%A6%AC)
