# CORS(Cross-Origin Resource Sharing)

자바스크립트는 기본적으로 same-origin policy(동일 출처 정책)를 따르며 실행중인 스크립트와 동일한 도메인에 있는 URL만 호출할 수 있다. 다른 출처(cross-origin)의 리소스가 필요한 경우,  CORS(교차 출처 리소스 공유)를 사용하면 다른 도메인에서 실행되는 API나 리소스의 액세스를 허용해 외부 스크립트와 에셋들을 사용할 수 있다.

<br>

### What is CORS?

CORS는 웹 페이지가 다른 페이지와 도메인의 에셋과 데이터를 사용하도록 허용하는 브라우저 메커니즘이다.

자바스크립트가 기본적으로 사용하는 동일 출처 정책은 외부 서버에 대한 액세스를 방지하는 가장 안전한 유형의 정책이다. 동일 출처 정책에서는 사이트의 모든 에셋들은 동일한 출처에서 가져와야하며 대부분의 스크립트들은 로컬 리소스로만 작동할 수 있다. 이를 통해 에셋들의 보안 위험을 완화한다.  

그러나 외부 에셋에 대한 액세스가 필요할때는 어떻게 해야할까, 이 경우 CORS를 통해 이 문제를 해결할 수 있다.

CORS는 웹 페이지와 호스트 서버가 상호 작용하는 방식을 정의하고 서버가 웹 페이지의 액세스 허용이 안전한지를 결정하는 정책이다. CORS를 통해 외부 에셋에 대한 액세스를 허용할 수 있다.

<br>

### What assets can CORS request?

웹 사이트는 CORS 요청을 사용해 아래와 같은 에셋들을 로드한다.

- fetch 요청 또는 `XMLHTTPRequests`와 같은 HTTP 요청
- 웹 폰트, TrueType 폰트
- WEB GL texture
- 이미지, 비디오
- CSS shapes

<br>

### How does CORS work?

CORS는 새로운 출처(origin)의 허용을 위해 표준 헤더 목록에 새로운 HTTP 헤더를 추가하며 이 헤더는 `Access-Control-Allow-Origin`이다.

그 외에도 다양한 종류의 응답 헤더가 존재하며 CORS HTTP 헤더의 종류는 아래와 같다.

- `Access-Control-Allow-Credentials`
- `Access-Control-Allow-Headers`
- `Access-Control-Allow-Methods`
- `Access-Control-Expose-Headers`
- `Access-Control-Max-Age`
- `Access-Control-Request-Headers`
- `Access-Control-Request-Method`
- `Origin`

<br>

![svg viewer](https://www.educative.io/api/page/4630276911661056/image/download/5886620109111296)

웹 브라우저가 사이트에 접근하려고 하면 브라우저는 사이트의 웹 서버에 CORS `GET` 요청을 보낸다. `GET` 요청이 승인되면 브라우저가 페이지를 볼 수 있도록 허용한다.

대부분의 서버들은 다양한 출처로부터의  `GET` 요청을 허용하지만 다른 종류의 요청은 차단한다.

서버는 요청된 데이터에 대한 접근이 제한되지않음을 의미하는 `*`(와일드카드)를 보내거나 허용된 출처 목록을 확인한다.

요청 출처가 목록에 있으면 웹 페이지를 볼 수 있으며 서버는 허용된 출처의 이름을 명시한다. 요청 출처가 목록에 없을 경우 서버는 모든 접근 및 특정 작업에서 허용되지 않는다는 내용을 나타내는 거절 메시지(declined message)를 반환한다. 

<br>

### Types of CORS request

요청 종류를 분리하면 출처의 수준을 명확하게 결정하고 각 출처가 필수적인 요청만을 수행할 수 있도록 할 수 있다.

대부분의 요청들은 아래와 같은 카테고리로 분류된다.

- simple requests : 본 요청전 확인(preflight check)을 실시하지 않으며 "safelisted" CORS 헤더만을 사용한다.
- preflight requests : 요청자가 요청전 수행할 작업에 대한 설명이 담긴 "preflight" 메시지를 보낸다. 요청을 받은 서버는 이 메시지를 검토해 요청이 안전한지를 확인한다.

<br>

<br>

### Simple requests

simple request는 본 요청전 확인을 필요로 하지 않는 `GET`, `POST`, `HEAD` 메소드 요청이다. 

<br>

#### `GET`

`GET` 요청은 특정 URL에서의 공유 데이터 파일에 대한 요청 및 다운로드를 위한 요청이다. 요청을 보내는 대상은 사이트의 콘텐츠를 보기만 할 수 있으며 텍스트 및 다른 요소에 대한 변경은 불가하다.

```http
GET /index.html
```

<br>

#### `POST`

`POST` 요청은 서버로의 데이터 전송을 요청한다. `POST` 요청이 여러번 트리거 될 경우 예기치못한 동작(unexpected behavior)들이 발생할 수 있다.

아래의 코드는 게시판에 댓글을 추가하기 위한 예제이다.

```HTTP
POST /test HTTP/1.1
Host: foo.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 27
field1=value1&field2=value2
```

브라우저는 입력 코멘트를 추가하라는 요청을 보낸다. 이 요청이 수락되면은 게시판 서버는 새로 수신된 코멘트를 가져와 다른 사람들이 볼 수 있도록 저장한다.

<br>

<br>

### Preflight requests

몇몇 메소드들은 본 요청전 전송되는 preflight 요청을 생성한다. preflight 요청은 사용자 데이터를 수정하거나 서버를 변경할 수 있는 기능을 가진  `OPTIONS` 메소드로 자동 생성된다.

`OPTIONS` 메소드는 클라이언트와 서버의 상호 작용을 위한 메소드에 대한 추가적인 정보를 수집하기위해 사용된다. 또한 액세스된 항목을 변경할 수 없는 안전한 방법이다.

`OPTIONS` 메소드는 수동으로 호출할 필요 없이 preflight 태그가 지정된 메소드를 요청하면 브라우저에서 자동으로 preflight 요청이 생성된다.

<br>

![svg viewer](https://www.educative.io/api/page/4630276911661056/image/download/6531045407588352)

가장 일반적은 preflight 메소드는 서버의 파일 또는 에셋 삭제를 위한 `DELETE`이다.

preflight 요청은 클라이언트가 요청한 출처와 사용하려는 메소드를 명시한 `Access-Control-Request-Method` 헤더를 포함한다. 서버는 이러한 preflight 요청을 분석해 해당 출처가 요청에 포함된 내용들을 수행할 수 있는 액세스 권한이 있는지 확인한다.

액세스 권한이 있을 경우 서버는 출처가 사용할 수 있는 모든 메소드들을 반환하고 본 요청을 보낼 수 있음을 클라이언트에게 보여준다. 액세스 권한이 없을 경우 이후 본 요청이 무시된다.

<br>

<br>

------

**Reference**

- [What is CORS (Cross-Origin Resource Sharing)?](https://www.educative.io/blog/getting-started-cors)
