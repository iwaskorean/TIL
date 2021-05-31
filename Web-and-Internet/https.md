# HTTPS

HTTPS(Hypertext Transfer Protocol Secure)는 브라우저와 웹 사이트간 데이터 전송에 사용되는 HTTP 프로토콜의 보안 버전이다.  데이터 전송시 보안성을 위해 암호화된다.



![i](https://www.cloudflare.com/img/learning/security/glossary/what-is-https/not-secure.png)

로그인 인증이 필요한 웹사이트들은 HTTPS를 사용해야하며 모던 웹 브라우저에서 HTTPS를 사용하지 않는 웹 사이트들은 HTTPS를 사용하는 웹 사이트와 다르게 표시된다.

<br>

### How does HTTPS work?

HTTPS는 통신은 암호화를 위해 암호화 프로토콜을 사용한다. 이것을 SSL(Secure Sockets Layer)라고 하며 [asmmetric public key infrastructure](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/)를 사용해 통신을 암호화한다. 이 유형의 보안 시스템은 통신 암호화를 위해 2가지 다른 key를 사용한다.

- private key : 웹 사이트의 소유자가 제어하며 private으로 유지된다. 이 키는 웹 서버에 있으며  public key로 암호화된 정보를 복호화하는데 사용된다.
- public key : 안전한 방식으로 서버와 interact한 작업을 하려는 모든 사람들이 사용할 수 있는 key다. public key로 암호화된 정보는 private key로 복호화할 수 있다.

<br>

### What happens if a website doesn’t have HTTPS?

정보가 HTTP를 통해 전송되면 데이터 패킷으로 나누어져 스누핑에 대상이 될 수 있다. HTTPS는 웹 사이트가 이러한 방식으로 정보가 브로드캐스트되는 것을 방지한다. 실제로 HTTP를 통해 발생하는 모든 통신은 일반 텍스트로 이루어지기 때문에 모든 사람들이 쉽게 액세스할 수 있으며 공격에 취약하다.

HTTPS를 사용하면 트래픽이 암호화되어서 패킷이 스니핑되거나 다른 방식으로 인터셉트되더라도 의미없는 문자들로 표시된다.

- 암호화 전

  ```
  This is a string of text that is completely readable
  ```

- 암호화 후

  ```
  ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0=
  ```



HTTPS가 없는 웹 사이트에서는 ISP(인터넷 서비스 제공자) 또는 중개자가 소유자의 승인 없이 웹 페이지에 컨텐츠를 추가할 수 있다. 이것은 주로 웹 사이트 내 광고에서 볼 수 있는데 ISP가 수익창출을 위해 고객의 웹 페이지에 광고를 삽입하는 형태다. 광고가 삽입되어 수익이 발생한다고 해도 웹 사이트의 소유자가 그 수익을 가져가는 것은 아니다. HTTPS는 이러한 형태의 제 3자가 웹 콘텐츠에 광고를 추가하는 기능을 제거한다.

<br>

### How is HTTPS different from HTTP?

기술적으로 HTTPS는 HTTP와 별개의 프로토콜은 아니다. HTTPS는 HTTP 프로토콜에 TLS/SSL 암호화를 사용하며 [TLS/SSL 인증서](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) 전송을 기반으로 통신함으로써 보안을 강화한다.

유저가 웹 페이지에 연결하면 웹 페이지는 보안 세션을 시작하기위해 pulic key를 포함한 SSL 인증을 보낸다. 그리고 클라이언트와 서버, 두 대의 컴퓨터는 보안 연결을 설정하는 SSL/TLS 핸드셰이크 프로세스를 통해 통신한다.

<br>

<br>

------

**Reference**

- [What is HTTPS?](https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/)
