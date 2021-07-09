# Next.js

Next.js는 SSR(server-side rendering), SG(static generation)와 같은 pre-rendering 기능을 통해 성능과 사용자 경험(user exprience)을 최적화할 수 있는 React 프레임워크이다.

<br>

## What is Next.js?

![I](https://www.educative.io/cdn-cgi/image/f=auto,fit=contain,w=1200/api/page/6452289848475648/image/download/5294398307303424)

Next.js는 SSR 및 SG와 같은 추가적인 최적화를 제공하는 오픈소스 리액트 프론트엔드 프레임워크이다. 

Next.js를 사용하면 React가 동작하는 방식인 CSR(client-side rendering)의 초기 렌더링 속도가 느리다는 단점과 SEO 문제점 등을 해결할 수 있다.

### SSR

Next.js에서는 다른 환경설정없이 SSR을 구현할 수 있다. 

SSR을 사용하면 서버가 모든 데이터에 엑세스하고 자바스크립트 코드를 처리해 페이지를 렌더링한다. 그 다음 브라우저로 다시 전송되고 즉시 렌더링된다. 따라서 빠른 시간내에 웹 페이지를 로드할 수 있다. 

### SEO

CSR은 빈 html 파일에 스크립트를 로딩하기 때문에 웹 사이트가 검색 결과에 노출되지 않는다. 그러나 SSR을 사용하면 웹 사이트가 검색 엔진 결과 페이지 상위에 노출되는 SEO 최적화가 가능하다. 

### `<head>` tag

Next.js에서는 React에서 할 수 없는 `<head>` 태그를 수정할 수 있다. `<head>` 태그는 웹 페이지의 metadata와 사이트의 SEO 랭킹에 주요한 부분을 차지한다.

<br>

## Why use Next.js?

Next.js의 주요 장점은 성능 향상과 SEO를 위한 빌트인 SSR 지원이다. 

SSR은 렌더링에 필요한 모든 정보를 서버로 보내는 요청 흐름으로 작동한다. 서버로 보내진 정보들을 통해 html 페이지를 pre-render 할 수 있으며 클라이언트가 서버에 단일 요청을 보내 html 페이지를 수신할 수 있다.

Next.js는 React 앱 보다 빠른 로드 속도, 내장 API, 페이지 라우팅, SEO 등의 장점이 있으며 최적화된 랜딩 페이지, 홈페이지 및 유기적인 검색 트래픽에 의존하는 페이지들에 특화되어있다. 

<br>

<br>

------

**Reference**

- [Next.js Tutorial with Examples: build better React apps with Next](https://www.educative.io/blog/nextjs-tutorial-examples)