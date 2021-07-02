# CORS(Cross-Origin Resource Sharing)

자바스크립트는 기본적으로 same-origin policy(동일 출처 정책)를 따르며 실행중인 스크립트와 동일한 도메인에 있는 URL만 호출할 수 있다.  

그러나 CORS(교차 출처 리소스 공유)는 다른 도메인에서 실행되는 API나 리소스의 액세스를 허용해 웹 페이지가 외부 스크립트와 에셋들을 사용하는 것을 가능하도록 해준다.

<br>

### What is CORS?

CORS는 웹 페이지가 다른 페이지와 도메인의 에셋과 데이터를 사용하도록 허용하는 브라우저 메커니즘이다.

동일 출처 정책은 외부 서버에 대한 액세스를 방지하는 가장 안전한 유형의 정책이다. 동일 출처 정책에서는 사이트의 모든 에셋들은 동일한 출처에서 가져와야하며 대부분의 스크립트들은 로컬 리소스로만 작동할 수 있다. 이를 통해 에셋의 보안 위험을 완화한다.  

그러나 외부 에셋에 대한 액세스가 필요할때는 어떻게 해야할까, 이 경우 CORS를 외부 에셋에 대한 액세스를 허용할 수 있다.

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

###### ...

###### How does CORS work?

###### Types of CORS requests

###### ...

<br>

------

**Reference**

- [What is CORS (Cross-Origin Resource Sharing)?](https://www.educative.io/blog/getting-started-cors)
