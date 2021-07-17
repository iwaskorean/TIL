# SSR, CSR

## Server-Side Rendering

SSR은 모든 페이지 리소스들이 브라우저가 아닌 서버에서 렌더링되는 기술이다. 서버는 요청이 발생하면 렌더링이 완료된 HTML, CSS, 자바스크립트 코드를 클라이언트로 전송한다. 

<br>

### How does SSR work?

SSR이 이루어지는 과정은 아래와 같다.

![ssr](https://cdn-bledd.nitrocdn.com/sMegMNkwPgLIyVZIoLggYdSAqxrSUlEd/assets/static/optimized/rev-67e96db/wp-content/uploads/2020/08/Picture1.jpg)



1. 사용자가 브라우저를 통해 웹 페이지를 요청한다.
2. 브라우저는 렌더링된 HTML, CSS 코드가 있는 서버에 연결한다.
3. HTML과 CSS가 사용자의 브라우저 표시된다. 그러나 아직 사용자와 인터랙션이 가능한 상태는 아니다.
4. 브라우저는 서버에서 자바스크립트를 다운로드한다.
5. 다운로드가 완료되면, 브라우저는 사용자와 인터랙션이 가능한 상태가 된다.
6. 브라우저는 자바스크립트를 실행한다.
7. 페이지가 완전히 로드되고 사용자의 인터랙션에 응답할 수 있다.

<br>

### Advantages of SSR

SSR의 장점은 다음과 같다.

- 빠른 초기 페이지 로드 시간 
- 개선된 SEO : 초기 로드 시간이 빠르기 때문에 검색 엔진 봇이 효율적으로 페이지를 크롤링하고 인덱싱할 수 있다.
- 인터넷 연결이 느린 사용자에 최적 : 자바스크립트가 처리되는 동안 렌더링된  HTML을 볼 수 있다.
- 소셜 미디어 최적화(SMO) : 페이지 링크를 복사해 다른 사람에게 전송하거나 SNS에 게시하면 페이지 제목, 설명, 이미지와 함께 미리보기가 표시된다.

<br>

### Disadvantages of SSR

SSR의 단점은 다음과 같다.

- 높은 서버 부하 : 모든 페이지 리소스들이 서버에 있고 브라우저가 서버에 계속해서 요청을 보낸다.
- 많은 시간 소모 : 큰 규모의 어플리케이션을 렌더링할 경우 서버에서 많은 시간이 소모된다.
- 인터랙션 : 페이지가 사용자에게 보여지는 동안 사용자는 브라우저와 인터랙션할 수 없다.
- 느린 TTTB(time to first byte) : 첫 요청에 대한 응답하기 위한 HTML 렌더링 처리에 시간이 소요된다.

<br>

## Client-Side Rendering(CSR)

CSR은 모든 페이지 리소스들이 서버가 아닌 클라이언트의 브라우저에서 렌더링되는 기술이다. CSR은 브라우저 측에서 코드를 컴파일하고 생성하는 동작 방식의 자바스크립트 프레임워크를 사용한다.

<br>

### How does CSR work?

CSR이 이루어지는 과정은 아래와 같다.

![CSR](https://cdn-bledd.nitrocdn.com/sMegMNkwPgLIyVZIoLggYdSAqxrSUlEd/assets/static/optimized/rev-67e96db/wp-content/uploads/2020/08/Picture2.jpg)

1. 사용자가 브라우저를 통해 웹 페이지를 요청한다.
2. 서버와 CDN은 요청에 대해 필수 자바스크립트 파일들의 링크를 포함하고 있는 HTML 페이지로 응답한다. 
3. 페이지는 아직 사용자에게 보여지지 않는다. 로더를 포함한 페이지라면 로더만 출력된다.
4. 브라우저는 HTML의 링크를 통해 자바스크립트를 다운로드한다.
5. 자바스크립트는 프레임워크를 통해 실행된다. 이 단계에서 사용자는 placeholder만 볼 수 있다.
6. 최종 렌더링을 위해 서버에 마지막 요청을 전송한다.
7. 이제 사용자는 페이지와 인터랙션할 수 있다.

<br>

### Advantage of CSR

CSR의 장점은 다음과 같다.

- 빠른 렌더링 속도 : 초기 렌더링 처리 속도는 느리지만, 그 이후의 렌더링들은 빠르게 처리된다.
- 웹 사이트의 빠른 탐색 제공 : 렌더링 시 placeholder가 먼저 로드되기 때문에 빠른 탐색이 가능하다.
- 적은 서버 부하 : 클라이언트의 브라우저에서 자바스크립트가 실행되므로 서버에는 비교적 적은 부하가 걸리게된다.
- 인터랙션

<br>

### Disadvantage of CSR

- 느린 로딩 속도 : HTML, CSS, 자바스크립트를 먼저 렌더링 한 다음 사용자에게 출력해야하기 때문에 첫 페이지 로딩 시간이 늘어난다.
- SEO : 검색 엔진 봇은 모든 페이지 리소스가 로드될 때 까지 기다려야 되기 때문에 크롤링, 인덱싱이 지연되고 SEO에 방해가된다.
- 외부 라이브러리에 의존한다.

<br>

<br>

------

**Reference**

- [Server-Side Rendering vs Client-Side Rendering – Which one to choose?](https://www.infidigit.com/blog/server-side-rendering-vs-client-side-rendering/)