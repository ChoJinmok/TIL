# Next.js의 Image 컴포넌트에 넣어줄 수 있는 Props
>Next.js를 사용하던 도중 'LCP', 'Fast Refresh will fall back to doing a full reload' warn을 만나고 priority Prop 알게됐다.

## 1. priority
- true인 경우 이미지가 '높은 우선 순위'와 '사전 로드'로 간주된다.(lazy loading 자동 비활성화)
- LCP(Large Contentful Paint) 요소로 감지된 모든 이미지에는 priority 속성을 넣어줘야 한다.
    - LCP 요소는 일반적으로 페이지의 뷰포트 내에서 볼 수 있는 가장 큰 이미지이다.
- 스크롤 없이 볼 수 있는 부분에 이미지가 표시되는 경우에만 사용해야 한다.
- Next.js가 로드할 이미지의 우선 순위를 지정할 수 있어 성능을 향상 시킬 수 있다.