# Next.js Image 컴포넌트 올바르게 사용하기

```ts
import Image from 'next/image';
```

Next.js의 Image 컴포넌트는 HTML <img> 요소의 확장된 형태이며 다음과 같은 기능을 제공한다.

- 최신 이미지 포맷을 사용해 뷰포트 맞춤 사이즈를 가진 이미지를 제공한다.
- 구글 SEO 순위에 핵심이 되는 [Cumulative Layout Shift](https://nextjs.org/learn/seo/web-performance/cls) 방지 기능을 제공한다.
- 뷰포트가 작은 장치에 큰 이미지를 보내는 것을 막을 수 있다.
- 뷰포트 외부에 위치한 이미지는 로드하지 않으며 해당 이미지에 스크롤 되었을 때 로드하는 지연 로드(lazy load) 기능을 제공한다.

이러한 기능들을 가진 Next.js의 Image 컴포넌트를 사용하면 간단히 이미지 최적화를 수행할 수 있다.

<br>

## Usage

### 정적 이미지

HTML img 요소와 마찬가지로 src 프로퍼티에 이미지 경로를 전달하면 된다.

### 외부 이미지 가져오기

외부 이미지를 가져오는 경우 다음 방법 중 하나를 선택해 수행해야 한다.

- `next.cofig` 파일 수정
- image loader 사용

1. `next.config` 파일 수정

- next.config.js 파일에 images 프로퍼티의 domain 프로퍼티를 설정해야 한다.

```ts
// pages/index.tsx

<Image
  src={'https://image.tmdb.org/t/p/original/asC6UOPrR6OlNMiyXbeXWu08SOG.jpg'}
  alt='alt Text'
  width={200}
  height={300}
/>
```

```js
// next.config.js

const nextConfig = {
	...
  images: {
    domains: ['image.tmdb.org'],
  },
	...
```

2. loader 사용하기

- loader는 외부 이미지의 url을 확인하는데 사용되는 함수다.
- Image 컴포넌트에 전달되는 src, width, quailty 프로퍼티의 값을 문자열로 반환해 전달한다.

```ts
const imageLoader = ({ src, width, quality }: ImageLoaderProps) => {
  return `https://image.tmdb.org/t/p/original${src}?w=${width}&q=${
    quality || 75
  }`;
};

/// ...

return (
  <Image
    loader={imageLoader}
    src={src}
    alt={'alt text'}
    width={500}
    height={700}
  />
);
```

### 필수 프로퍼티

Image 프로퍼티의 필수 프로퍼티는 다음과 같다.

- src
- height, width 또는 layout="fill"

만약 src 프로퍼티만 전달할 경우 다음과 같은 에러가 발생한다.

```
Image with src "www.srcExample.com" must use "width" and "height" properties or "layout='fill'" property.
```

<br>

## Props

### width, height

- 이미지의 픽셀 너비, 높이를 나타낸다.
- 단위를 제외한 정수형으로 입력하며 픽셀 단위로 렌더링된다.

### layout

layout 프로퍼티는 뷰포트에 따른 이미지 레이아웃 변경을 수행한다.

- fixed: 뷰포트가 변해도 이미지 크기가 변하지 않는다.
- intrinsic: 기본값이며 작은 뷰포트에서 크기가 줄어든다.
- reponseive: 작은 뷰포트에서 크기가 줄어들고 큰 뷰포트에서 크기가 커진다.
- fill: 부모 요소에 맞춰 크기가 결정된다.

### objectFit, ObjcetPosition

- 두 프로퍼티 모두 주로 `layout: 'fill'`과 함께 사용된다.
- objectFit 프로퍼티의 값은 css object-fit 프로퍼티에 전달된다.

### quailty

- 이미지 최적화 품질로 최대값은 100, 최소값은 1이다.
- 75가 기본값이다.

### priority

- 기본값은 false이며 true인 경우 높은 우선 순위로 미리 로드(preload) 된다.
- 최상단에 위치한 이미지에 사용이 권장된다.

### placeholder

- empty인 경우 이미지가 로드되는 동안 아무것도 보여주지 않는다.
- blur인 경우 blurDataURL의 이미지로 채워진다.

### blurDataURL

- placeholder 프로퍼티가 blur인 경우 이미지가 로드될 동안 placeholder로 사용할 이미지의 경로를 전달한다.
- 확대되어 흐려진 형태로 표시된다.

### loading

- 이미지 로딩에 대한 수행 방식을 결정하며 lazy를 기본값으로 갖는다.
- lazy인 경우 지연 로드를 수행하며 eager인 경우 즉시 로드한다.

<br>

---

### Reference

- [Image Component and Image Optimization](https://nextjs.org/docs/basic-features/image-optimization)
- [next/image](https://nextjs.org/docs/api-reference/next/image)
