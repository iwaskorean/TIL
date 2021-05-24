# HTTP Overview

HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있게 해주는(fetch) 프로토콜이다.  HTTP는 웹에서의 모든 데이터 교환들의 토대이며 수신자에 의해 요청이 초기화되는 클라이언트-서버 프로토콜이다.



![IMG](https://mdn.mozillademos.org/files/13677/Fetching_a_page.png)

클라이언트와 서버들은 개별적인 메시지들을 교환함으로써 통신한다. 보통 브라우저인 클라이언트가 보내는 메시지를 요청(request)이라고하고 서버에서 응답으로 보내는 메시지를 응답(response)라고 한다.

<br>

### Components of HTTP-based systems

각각의 개별적인 요청은 서버로 보내지고 서버는 요청에 대한 응답(response)를 제공한다. 요청과 응답사이에 많은 엔터티들이 있는데 게이트웨이나 캐시와 같이 다양한 역할을 하는 프록시가 그 중 하나이다.

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/client-server-chain.png)

### Client: the user agent

유저 에이전트는 유저를 대신해 동작하는 도구이다. 주로 브라우저가 그 역할을 한다. 브라우저는 항상 요청을 보내는 엔터티이며 서버가 될 수 없다. 

브라우저는 웹페이지를 나타내기위해  HTML 문서를 가져오기 위한 요청을 전송하고 파일을 분석해 실행할 스크립트, 이미지와 비디오 등의 하위 리소스를 출력하기 위한 레이아웃 정보(CSS)에 대응하는 추가적인 요청을 가져온다. 웹 브라우저는 이러한 리소스들을 혼합해 완전한 문서인 웹 페이지를 제공한다.

웹 페이지는 하이퍼텍스트 문서이다. 하이퍼텍스트 문서란 출력된 텍스트의 일부가 유저가 유저 에이전트를 조작하고 웹을 돌아다닐수 있도록 새로운 웹 페이지를 가져오기위해 실행될 수 있는 링크를 뜻한다.  브라우저는 HTTP 요청에서의 부분들을 변환하고 HTTP 응답을 해석해 유저에게 명확한 응답을 표시한다.

### The Web server

통신 채널의 반대편에는 클라이언트에 의해 요청된 문서들을 제공하는 서버가 있다.  여러 개의 서버를 동일한 기계에서 호스팅 할 수 있다. HTTP/1.1과 `Host` 헤더를 이용해 동일한 IP 주소를 공유할  있다.

### Proxies

웹 브라우저와 서버 사이에 많은 컴퓨터과 기계들을 HTTP 메시지들이 전달된다. 웹 스택의 계층 구조 중 네트워크 또는 물리 계층에서 대부분의 동작과 전송이 일어나며 성능에 큰 영향을 주지만 HTTP 계층에서는 이러한것들이 어떻게 동작하는지 보이지 않는다. 

어플리케이션 계층에서의 이러한 동작들을 프록시라고 부르며 아래와 같은 기능들을 수행할 수 있다.

- 캐싱 
- 필터링 : 백신 스캔, 자녀 보호 기능
- 로드 밸런싱 : 여러 서버들이 다른 요청들을 처리하도록 허용
- 인증(authentication) : 다른 리소스들에 대한 액세스 제어
- 로깅 : 기록 정보에 대한 저장 허용

<br>

### Basic aspects of HTTP

- #### HTTP는 간단하다.

  HTTP는 사람이 읽고 이해할 수 있게 설계되어 개발자들을 위한 쉬운 테스트를 지원하고 초심자들을 위해 복잡한 부분들이 제외되었다.

- #### HTTP는 확장 가능하다.

  HTTP/1.0의 HTTP 헤더를 사용하면 HTTP를 확장하고 실험하기 간편하다. 클라이언트와 서버가 새로운 헤더의 시멘틱에 대해 이해하고 있다면 새로운 기능을 추가할 수 있다.

- #### HTTP는 상태를 저장하진 않지만(stateless) 세션이 존재한다.(not sessionless)

  HTTP는 상태를 저장하지 않는다. 그러나 헤더의 확장성과 HTTP 쿠키를 통해 동일 컨텍스트와 상태를 공유하기 위해 각각의 HTTP 요청들을 세션으로 만들수 있다.

- #### HTTP와 연결

  HTTP/1.0에서의 TCP 프로토콜 연결의 비효율성을 개선하기위해 HTTP/1.1에서는 파이프라인 개념과 지속적인 연결 개념을 도입했다.HTTP/2는 연결의 지속성과 효율성을 유지하기 위한 부분들이 도입되었으며 발전된 전송 프로토콜의 설계를 위한 실험들이 이루어지고 있다.

<br>

### What can be controlled by HTTP

HTTP를 통해 제어할 수 있는 기능들은 아래와 같다.

- 캐시 : HTTP로 문서가 캐시되어지는 방식에 대한 제어
- origin 제약 완화 : HTTP를 헤더를 통한 origin 제약사항 완화
- 인증 : HTTP 헤더와 쿠키를 통해 특정 세션을 설정
- 프록시와 터널링 
- 세션 : HTTP가 stateless임에도 세션을 만들도록 쿠키를 사용해 요청을 서버 상태와 함께 연결하도록 허용

<br>

### HTTP flow

1. TCP 연결을 개방한다.

2. HTTP 메시지를 전송한다.

   ```
   GET / HTTP/1.1
   Host: developer.mozilla.org
   Accept-Language: fr
   ```

3. 서버가 전송한 응답을 읽는다.

   ```
   HTTP/1.1 200 OK
   Date: Sat, 09 Oct 2010 14:28:02 GMT
   Server: Apache
   Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
   ETag: "51142bc1-7449-479b075b2891b"
   Accept-Ranges: bytes
   Content-Length: 29769
   Content-Type: text/html
   
   <!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
   ```

4. 연결을 종료하거나 다른 요청들을 위해 재사용한다.

<br>

### HTTP Messages

HTTP 메시지의 두 가지 타입인 요청과 응답은 각자의 포맷을 가지고 있다.

#### 요청(request)

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_request.png)

- HTTP Method : 클라이언트가 실행하고자 하는 동작인 `GET`, `POST`, `OPTIONS`, `HEAD`

- Path : 프로토콜, 도메인, TCP 포트등을 제거한 리소스의 URL

- Version of the protocol : HTTP 버전 정보

- Headers : 서버에 대한 추가적인 정보 전달

  

#### 응답(response)

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_response.png)

- Version of the protocol : HTTP의 버전 정보
- Status code : 요청의 성공여부를 나타내는 코드
  - 1xx : 조건부 응답, 요청을 받았으며 작업을 계속함
  - 2xx : 성공, 200(서버가 요청한 페이지 제공), 201(서버가 새 리소스 작성) ...
  - 3xx : 리다이렉션 완료
  - 4xx : 요청 오류, 404(서버가 요청한 페이지를 찾을 수 없다.) ... 
  - 5xx : 서버 오류
- Status message : 상태 코드에 대한 메시지
- Headers : 요청 헤더와 마찬가지로 추가적인 정보 내역

<br>

### Conclusion

HTTP는 사용이 쉬운 확장 가능한 프로토콜이다. 헤더를 추가하는 기능과 결합된 클라이언트-서버 구조를 통해 HTTP를 웹의 확장된 기능들과 함께 사용함으로써 다양한 기능을 동작하게할 수있다.

HTTP/2에서 성능 향상을 위해 메시지를 프레임 내부로 내장시켜 복잡성이 증가했지만 메시지 기본 구조는 HTTP/1.0 이후와 동일하다. 또한 세션은 여전히 단순하게 조작할 수 있기 때문에 HTTP 메시지 모니터를 통한 분석과 디버깅이 가능하다.

<br>

<br>

------

**Reference**

- [An overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [HTTP 상태 코드](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)

