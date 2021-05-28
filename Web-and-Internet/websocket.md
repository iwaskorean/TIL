# WebSocket

WebSocket은 양방향(bidirectional) 전이중(full-duplex) 방식의 채널을 제공해 클라이언트와 서버간 메시지 전달을 용이하게하는 상태 저장 통신 프로토콜이다. 

<br>

### Why WebSocket?

WebSocket의 아이디어는 HTTP 기반 기술의 한계로부터 시작되었다. 

HTTP는 단방향 프로토콜이며 클라이언트가 리소스를 요청하고 서버는 요청된 데이터를 응답한다. 클라이언트가 먼저 요청을 해야만 서버에서 클라이언트로 데이터가 전송된다. 긴 폴링(long-poling)을 사용해 긴 시간 제한을 가진 HTTP 요청을 만들고 서버가 긴 시간 제한을 사용해 데이터를 클라이언트로 보냄으로써 단방향 프로토콜의 제약을 해결하려고 했지만 전송할 데이터가 없는 경우에도 서버가 긴 폴링 시간 동안 묶여있다는 단점은 해결하지 못했다.

반면 WebSocket은 UDP와 유사하지만 TCP의 신뢰성을 가진 메시지 기반의 데이터를 보낼 수 있다. WebSocket은 HTTP 초기 전송 방식을 사용하지만 HTTP 응답을 받은 후 TCP 연결을 유지해 클라이언트와 서버간 메시지 전송을 할 수 있다. 따라서 WebSocket을 사용하면 긴 폴링의 사용없이 실시간 어플리케이션을 빌드할 수 있다.

<br>

### Protocol Overview

WebSocket은 표준 HTTP 요청과 응답으로 시작된다. 요청/응답 체인에서 클라이언트는 WebSocket 연결을 열도록 요청하고 연결이 가능한 경우 서버가 응답한다. 이 초기 handshake가 성공하면 클라이언트와 서버는 TCP/IP 연결을 WebSocket 연결로 사용하는데 합의한 것이다. 이제 데이터는 기본 프레임 메시지 프로토콜을 사용해 통신할 수 있으며 양쪽이 WebSocket 연결을 닫으면 TCP 연결이 끊어진다.

<br>

### Establishing a WebSocket connection - Open Handshake

WebSocket은 HTTP 프로토콜을 따르지않기 때문에 `http://` 또는 `https://` 스키마를 사용하지 않는다. WebSocket URI는 `ws:` 또는 `wss: ` 스키마를 사용한다. 나머지 URI는 HTTP URI와 동일하다

```
"ws:" "//" host [ ":" port ] path [ "?" query ]
"wss:" "//" host [ ":" port ] path [ "?" query ]
```

WebSocket 연결을 위해 클라이언트는 `Connection: Upgrade` , `Upgrade: websocket`, `Sec-WebSocket-Key`, `Sec-WebSocket-Version: 13` 헤더가 포함된 HTTP 요청을 보낸다.

```
// request by client
GET ws://example.com:8181/ HTTP/1.1
Host: localhost:8181
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: q4xkcO32u266gldTuKaSOw==
```

클라이언트가 WebSocket 연결을 열기 위해 위와 같은 초기 요청을 보내면 서버는 `Upgrade` 요청 헤더와 연결이 성공적으로 Upgrade 되었다는 것을 확인하는`Connection` 헤더를 포함한 응답을 보낸다

```
// response by server
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: fA9dggdnMPU79lJgAE3W4TRnyDM=
```

클라이언트가 서버의 응답을 수신하면 WebSocket 연결이 개방되고 데이터 전송이 시작된다.

<br>

### Closing a WebSocket connection - Close Handshake

WebSocket 연결을 닫기위해서 클로징 프레임이 전송된다. 클로즈 프레임에는 opcode 및 연결을 닫는 이유를 나타내는 본문(body)이 포함된다. 서버 양측이 클로즈 프레임을 수신하면 다시 응답으로 클로즈 프레임을 보내야하며 더이상 데이터를 전송해서는 안된다. 양측이 응답 클로즈 프레임을 수신하면 TCP 연결이 끊어지며 WebSocket 연결이 종료된다.

<br>

### **Differences between HTTP and WebSocket Connection**

- #### WebSocket Connection

  ![1](https://media.geeksforgeeks.org/wp-content/uploads/20191203183648/WebSocket-Connection.png)

  - 양방향 프로토콜이며 클라이언트나 서버가 종료될떄까지 연결이 유지된다.

  - 대부분의 실시간 어플리케이션(거래, 모니터링, 알림 등) 서비스에 사용되며 WebSocket을 사용해 단일 통신 채널에서 데이터를 수신한다. 



- #### HTTP Connection

  ![2](https://media.geeksforgeeks.org/wp-content/uploads/20191203183429/HTTP-Connection.png)

  - 단방향 프로토콜이며 HTTP 연결이 닫혔다는 응답을 받은 후 HTTP 요청 메소드를 사용해 연결을 만들 수 있다.

  - 간단한 RESTful 어플리케이션에 사용되며 HTTP 프로토콜을 사용해 데이터를 수신한다.

<br>

<br>

------

**Reference**

- [How Do Websockets Work?](https://sookocheff.com/post/networking/how-do-websockets-work/)
- [What is web socket and how it is different from the HTTP?](https://www.geeksforgeeks.org/what-is-web-socket-and-how-it-is-different-from-the-http/)
