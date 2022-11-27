# Assets, Metadata, and CSS

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Assets, Metadata, and CSS](https://nextjs.org/learn/basics/assets-metadata-css)내용들을 정리했습니다.

<br />

## 1. Assets

Next.js는 **최상위 디렉토리인 [`public`](https://nextjs.org/docs/basic-features/static-file-serving)**을 통해 이미지와 같은 **정적 asset**을 제공한다. `public` 내부의 파일은 [`pages`](https://nextjs.org/docs/basic-features/pages)와 유사하게 응용 프로그램의 루트에서 참조할 수 있다.

`public` 디렉토리는 `robots.txt`, Google Site Verification 및 기타 정적 asset에도 유용하다. 자세한 내용은 [정적 파일 제공](https://nextjs.org/docs/basic-features/static-file-serving)에 대한 설명서를 확인

### 1.1. Download Your Profile Picture

먼저 프로필 사진을 검색

- 프로필 사진을 `.jpg` 형식으로 다운로드(혹은 [이 파일을 사용](https://github.com/vercel/next-learn/blob/master/basics/basics-final/public/images/profile.jpg))
- [`public` 디렉토리](https://nextjs.org/docs/basic-features/static-file-serving) 내에 `images` 디렉토리를 만든다.
- 사진을 `public/images` 디렉토리에 `profile.jpg`로 저장
- 이미지 크기는 약 400x400px
- [`public` 디렉토리](https://nextjs.org/docs/basic-features/static-file-serving)에서 직접 사용하지 않는 SVG 로고 파일을 제거

### 1.2. Unoptimized Image

일반 HTML을 사용하면 다음과 같이 프로필 사진을 추가

```html
<img src="/images/profile.jpg" alt="Your Name" />
```

다음은 수동으로 처리:

- 다양한 화면 크기에서 이미지가 반응하는지 확인
- 타사 도구 또는 라이브러리로 이미지 최적화
- 뷰포트에 들어갈 때만 이미지를 로드

Next.js는 이를 처리해주는 `Image` component를 제공한다.

### 1.3. Image Component and Image Optimization

[`next/image`](https://nextjs.org/docs/api-reference/next/image): 현대 웹을 위해 진화된 HTML `<img>` element

Next.js는 기본적으로 이미지 최적화도 지원한다. 이를 통해 [WebP](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#webp)와 같은 최신 형식으로 이미지 크기 조정, 최적화 및 제공이 가능하다(브라우저가 지원가능 해야함). 이렇게 하면 뷰포트가 작은 장치에 큰 이미지가 전달되는 것을 방지할 수 있다. 또한 Next.js는 향후 이미지 형식을 자동으로 채택하고 해당 형식을 지원하는 브라우저에 제공할 수 있다.

자동 이미지 최적화는 모든 이미지 소스에서 작동한다. 이미지가 CMS와 같은 외부 데이터 소스에서 호스팅되는 경우에도 여전히 최적화할 수 있다.

### 1.4. Using the Image Component

빌드 시 이미지를 최적화하는 대신 Next.js는 사용자가 요청할 때 주문형으로 이미지를 최적화한다. 정적 사이트 생성기 및 정적 전용 솔루션과 달리 10개의 이미지를 배송하든 1천만 개의 이미지를 배송하든 빌드 시간이 늘어나지 않는다.

이미지는 기본적으로 지연 로드된다. 즉, 표시 영역 외부의 이미지에 대해 페이지 속도가 저하되지 않는다. 이미지가 뷰포트로 스크롤되면서 로드된다.

이미지는 항상 Google이 [검색 순위에 사용](https://developers.google.com/search/blog/2020/05/evaluating-page-experience)할 [Core Web Vital](https://web.dev/vitals/#core-web-vitals)인 [누적 레이아웃 이동](https://web.dev/cls/)을 피하는 방식으로 렌더링된다.

다음은 프로필 사진을 표시하기 위해 [`next/image`](https://nextjs.org/docs/api-reference/next/image)를 사용하는 예이다. `height` 및 `width` props는 소스 이미지와 동일한 종횡비로 원하는 렌더링 크기여야 한다.

```tsx
import Image from "next/image";

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
);
```

Next.js는 [CSS](https://nextjs.org/docs/basic-features/built-in-css-support)와 [Sass](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support)를 기본적으로 지원한다.
