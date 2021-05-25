# HTTP Messages

> *이전 포스트 : [HTML Overivew](https://github.com/SewookHan/TIL/blob/main/Web-and-Internet/http.md)*


HTTP 메시지는 서버와 클라이언트가 데이터를 교환하는 방식이다. 메시지에는 요청(request)와 응답(response)가 있으며 요청은 클라이언트가 특정 동작을 위해 서버로 보내는 메시지이며 응답은 요청에 대한 서버의 회신이다.



![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsg2.png)



HTTP 메시지는 ASCII코드로 인코딩된 문자 정보들로 구성되어 있다. 

- HTTP 프로토콜 초기 버전 및 HTTP/1.1
  - 메시지들이 개방적으로 전달
  - 사람이 읽을 수 있는 형태

- HTTP/2
  - 이진 프레이밍 메커니즘(binary framing mechanism) : API나 config 파일의 변경 불필요
  - 최적화와 성능 향상을 위해 HTTP 프레임으로 분할된 형태

HTTP 메시지는 config 파일(프록시나 서버) , API(브라우저) 또는 다른 인터페이스를 통해 제공된다.



![img](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsgstructure2.png)

HTTP 요청과 응답은 비슷한 구조를 가졌으며 시작 줄(start-line), 헤더, 빈 줄(blank-line),  본문(body)로 이루어져있다.

<br>

## HTTP Request

### Start line

시작 줄은 아래와 같은 3가지 요소를 포함한다.

1. HTTP 메소드 
   - `GET`, `PUT`, `POST`와 같은 동사 및 `HEAD`, `OPTIONS`를 사용해 서버가 수행할 동작을 나타낸다. 예를들어 `GET`은 리소스를 fetch 하는 동작을 나타내며 `POST`는 데이터가 서버로 push 되는 동작을 의미한다.

2. 요청 타겟
   - 주로 URL이며 프로토콜, 포트, 도메인의 절대경로로 나타낼수도 있다. 요청 타겟은 요청 컨텍스트에 의해 특정되어진다.

3. HTTP 버전
   - 버전에 따라 메시지의 나머지 부분의 구조가 결정된다.



### Headers

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/http_request_headers3.png)

헤더는 `:`으로 구분되며 대소문자 구분 없는(case-insensitive) 문자열로 이루어져있다.  헤더에 따라 뒤에오는 값이 달라진다. 헤더는 값을 포함한 한줄로 구성되지만 꽤 긴 형태를 가질수도 있다.

요청시 다양한 종류의 헤더가 있으며 아래와 같은 그룹으로 나눌수 있다.

- Request 헤더 : 요청을 구체화(`User-Agent`, `Accept-Type`, `Accept-Language`), 컨텍스트 제공(`Referer`), 조건부 제한(`If-None`)
- General 헤더 : 전체 메시지에 적용되는 헤더(`Via`)
- Representation 헤더 : 메시지 데이터 원래의 포맷을 나타내며 인코딩 적용(`Content-Type`)



### Body

본문은 요청의 마지막 부분이며 모든 요청이 본문 부분을 가지는 것은 아니다. 리소스를 가져오는 요청(`GET`, `HEAD`, `DELETE` 등)은 본문 포함하지 않아도된다. 

본문은 아래와 같은 두가지 종류로 나뉜다.

- Single-resource 본문 : 헤더 두개(`Content-Type`, `Content-Length`)로 나누어진 단일 파일
- multiple-resource 본문 : 각각 다른 정보들을 포함한  멀티파트로 구성된 본문. 보통 HTML Form과 관련이 있다.

<br>

## HTTP Responses

### Status line

`HTTP/1.1 404 Not Found`

HTTP 응답의 시작 줄은 상태 줄이라고 하며, 아래와 같은 정보를 포함한다.

1. 프로토콜 버전, 보통 `HTTP/1.1` 버전이다
2. 상태 코드(status code): `200`, `404`와 같은 요청의 성공 여부를 나타낸다.
3. 상태 텍스트(status text) : 사람들이 HTTP 메시지를 이해하기 쉽게 하기 위한 상태 코드에 대한 설명이다.



### Header

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/http_response_headers3.png)

헤더는 `: `으로 구분되며 대소문자 구분 없는(case-insensitive) 문자열로 이루어져있다.  헤더에 따라 뒤에오는 값이 달라지며  값을 포함한 한줄로 표시된다.

다양한 응답 헤더가 있으며 아래와 같은 그룹으로 나눌수 있다.

- Response 헤더 : 상태 줄에 포함되지 않은 서버에 대한 부가 정보 제공(`Vary`, `Accept-Ranges`)
- General 헤더 : 전체 메시지에 적용되는 헤더(`Via`)
- Representation 헤더 : 메시지 데이터 원래의 포맷을 나타내며 인코딩 적용(`Content-Type`)



### Body

본문은 응답의 마지막 부분이며 `201`, `204`와 같은 상태코드를 가진 응답은 본문을 필요로하지 않는다.

본문은 아래와 같은 세가지 종류로 나뉜다.

- 길이를 알고 있는 Single-resource 본문 : 헤더 두개(`Content-Type`, `Content-Length`)로 나누어진 단일 파일
- 길이를 모르는 Single-resource 본문 : 청크들로 인코딩된 단일 파일
- multiple-resource 본문 : 각각 다른 정보들을 포함한  멀티파트로 구성

<br>

## HTTP/2 Frames

![i](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/binary_framing2.png)

HTTP/1.X 버전에서의 메시지는 헤더가 압축되지 않으며 비슷한 헤더 구조의 반복 전송, 서버 하나에 연결을 여러개 열어야하는 다중전송의 불가 등의 문제점이 있다.

그러나 HTTP/2에서는 메시지를 프레임으로 나누어 데이터와 헤더프레임이 분리되었기 때문에 헤더를 압축할 수 있으며 멀티플렉싱(스트림 여러개를 하나로)을 통해 TCP의 효율적인 연결이 가능하다.

<br>

<br>

## Examples

### Example 1

hello.htm 페이지를 가져오기 위한 HTTP 요청

```HTML
<html>
   <body>

   <h1>Hello, World!</h1>

   </body>
</html>
```

- Client request

  ```HTTP
  GET /hello.htm HTTP/1.1
  User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
  Host: www.tutorialspoint.com
  Accept-Language: en-us
  Accept-Encoding: gzip, deflate
  Connection: Keep-Alive
  ```

- Server response

  ```HTTP
  HTTP/1.1 200 OK
  Date: Mon, 27 Jul 2009 12:28:53 GMT
  Server: Apache/2.2.14 (Win32)
  Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
  Content-Length: 88
  Content-Type: text/html
  Connection: Closed
  ```



### Example 2

tutorialspoint.com에서 실행중인 웹 서버의 존재하지 않는 t.html 페이지를 가져오는 HTTP 요청

```HTML
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>

<head>
   <title>404 Not Found</title>
</head>

<body>
   <h1>Not Found</h1>
   <p>The requested URL /t.html was not found on this server.</p>
</body>

</html>
```

- Client request

  ```HTTP
  GET /t.html HTTP/1.1
  User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
  Host: www.tutorialspoint.com
  Accept-Language: en-us
  Accept-Encoding: gzip, deflate
  Connection: Keep-Alive
  ```

- Server response

  ```HTTP
  HTTP/1.1 404 Not Found
  Date: Sun, 18 Oct 2012 10:36:20 GMT
  Server: Apache/2.2.14 (Win32)
  Content-Length: 230
  Content-Type: text/html; charset=iso-8859-1
  Connection: Closed
  ```



### Example 3

tutorialspoint.com에서 실행중인 웹 서버의 hello.htm 페이지를 가져오기 위한 요청시 잘못된 HTTP 버전 작성에 대한 서버 응답

```HTML
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>

<head>
   <title>400 Bad Request</title>
</head>

<body>
   <h1>Bad Request</h1>
   <p>Your browser sent a request that this server could not understand.<p>
   <p>The request line contained invalid characters following the protocol string.<p>
</body>

</html>
```

- Client request

  ```HTTP
  GET /hello.htm HTTP1
  User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
  Host: www.tutorialspoint.com
  Accept-Language: en-us
  Accept-Encoding: gzip, deflate
  Connection: Keep-Alive
  ```

- Server response

  ```HTTP
  HTTP/1.1 400 Bad Request
  Date: Sun, 18 Oct 2012 10:36:20 GMT
  Server: Apache/2.2.14 (Win32)
  Content-Length: 230
  Content-Type: text/html; charset=iso-8859-1
  Connection: Closed
  ```

<br>

<br>

------

**Reference**

- [HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)
- [HTTP - Message Examples](https://www.tutorialspoint.com/http/http_message_examples.htm)
