# Domain Name

### What is a Domain Name?

도메인 이름이란 웹 사이트를 방문하기 위해 브라우저 URL 표시줄에 입력하는 웹 사이트의 주소이다. 간단히 말해서 웹 사이트가 집이라면 도메인 이름은 그 집의 주소가 되는 것이다.

인터넷은 컴퓨터들끼리 통신하기 위해 연결된 거대한 글로벌 네트워크이며 각 컴퓨터를 식별하기 위해 숫자와 `.` 으로 구성된 IP주소가 할당된다. 이러한 형태의 IP 주소는 기억하기 어렵다는 문제점이 있다. 이 문제점을 해결하기 위해 도메인 이름이 만들어졌다.

<br>

### How does Domain Name Actually work?

![i](https://www.wpbeginner.com/wp-content/uploads/2017/09/howdomainswork.png)



웹 브라우저에서 도메인 이름을 입력하면 먼저 DNS(Domain Name System)를 구성하는 글로벌 네트워크로 요청을 보낸다. 서버는 도메인과 관련된 네임 서버(name servers)를 찾아서 해당 네임 서버로 요청을 전달한다.

예를 들어, 웹사이트가 Bluehost에서 호스팅되는 경우 네임 서버는 `ns1.bluehost.com`, `ns2.bluehost.com`과 같은 형태를 가지게 된다. 이러한 네임 서버들은 호스팅 회사에서 관리하는 컴퓨터이며 호스팅 회사는 웹 사이트가 저장된 컴퓨터로 요청을 전달한다.

이런 컴퓨터를 웹 서버라고 한다. 웹 서버에는 특수한 소프트웨어가 설치되어 있으며(Apache, Nginx), 웹 페이지 및 관련된 정보들을 가져오는 역할을 한다. 

마지막으로 이 데이터를 브라우저로 보냄으로써 사용자가 웹 페이지에 접속할 수 있게 된다.

<br>

### How is Domain Name Different from a Website and Web Hosting?

도메인 이름이 웹 사이트의 주소라면 웹 호스팅은 웹 사이트가 있는 집이다. 웹 사이트의 파일이 저장되는 실제 컴퓨터를 서버라고하며 호스팅 회사에서 이 서버를 제공하는 것이다.

웹 사이트를 만들기위해서는 도메인 이름과 웹 호스팅 모두 필요하다. 동일한 회사에서 이 두 서비스를 함께 구매하는 것이 좋으며 만약 다른 회사에서 구매할 경우 도메인 이름 설정을 수정하고 호스팅 회사에서 제공한 네임 서버 정보를 입력해야한다. 네임 서버 정보는 도메인 이름에 대한 사용자의 요청을 보낼 위치를 정의한다.

<br>

### Domain Name Space

![1](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b1/Domain_name_space.svg/440px-Domain_name_space.svg.png)



인터넷 주소자원 관리기관(ICANN, Internet Corporation for Assigned Names and Numbers)은 도메인 이름 시스템을 관리하며 정책을 만드는 비영리 단체이다. ICANN은 도메인 등록 및 할당 할 수 있는 DNR(domain name registers) 권한을 도메인 기관에 부여한다. 권한을 부여받은 기관은 도메인 이름을 판매할 수 있으며 사용자를 대신해 DNR를 변경할 수 있다.



도메인 이름은 기술적으로 label들로 구성되며 `.`으로 구분된다. 예를 들어 `www.example.com`은 최상위 도메인(TLD)인 .com에 포함되며 오른쪽에서부터 왼쪽으로 계층 구조가 형성된다.  example.com의 하위 레이블인 www는 `www.example.com`을 생성하기 위한 서브 레이블이다.

최상위 도메인(TLD) 중 가장 인기있는 것은 .com 이다. 그 외에도 .org, .net, .info, .io 등이 있다. 그러나 일반적으로 상업용 사이트를 나타내는 .com 도메인 확장자를 사용하는 것이 좋다. 

최상위 도메인은 다양한 유형이 존재한다.

- TLD(Top Level Domain) 
  - 도메인 이름 시스템에서 가장 높은 레벨에 있는 도메인 확장자이다.
  - 가장 있는 것은 .com, .net 등이며 그 이외의 .biz, .club, .info 등은 잘 알려져있지 않기 때문에 사용하지 않는 것이 좋다.

- ccTLD(Country Code Top Level Domain)
  - .uk(영국), .kr(대한민국), .in(인도) 등 국가 코드 확장자로 끝나는 국가별 도메인 이름이며 국가의 NIC에서 관리한다.

- sTLD(Sponsored Top Level Domain)
  - 특정 단체를 대표하는 도메인 확장자이다. 교육 관련 단체는 .edu, 미국 정부의 경우 .gov를 사용한다.

<br>

<br>

## DNS(Domain Name System)

DNS는 인터넷의 전화번호부와 같은 역할이다. DNS는 호스트의 도메인 이름을 IP 주소로 변환해 인터넷 리소스를 불러오기 위한 시스템이다.

<br>

### 4 DNS servers involved in loading a webpage

웹 페이지를 로딩할때 4개의 DNS 서버가 필요하다.

- DNS recursor : 웹 브라우저와 같은어플리케이션을 통해서 클라이언트 컴퓨터로부터 쿼리를 받도록 설계된 서버

- Root name server : 사람이 읽을 수 있는 도메인 이름을 IP 주소로 변환

- TLD name server : 특정 IP 주소 검색 다음의 단계이며 도메인 이름의 마지막 부분인 최상위 도메인을 호스팅

- Authoritative name server : 네임 서버 쿼리의 마지막 도착지이며 요청한 레코드의 대한 액세스 권한이 있다면 요청한 호스트 이름의 IP 주소를 DNS recursor에게 돌려 보냄 

<br>

### The 8 steps in a DNS lookup

![1](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)



1. 사용자가 브라우저에 example.com을 입력하면 쿼리가 인터넷으로 이동하고 DNS recursor가 수신한다.
2. resolver가 DNS root 네임 서버를 쿼리한다.
3. 루트 서버가 도메인에 대한 정보를 가지고 있는 TLD DNS 서버(.com, .net 등)의 주소로 resolver에게 응답한다.
4. resolver가 .com TLD에게 요청을 보낸다.
5. TLD 서버는 도메인 네임 서버의 IP 주소를 resolver에게 응답한다.
6. 마지막으로 recursive resolver가 도메인 네임 서버로 쿼리를 보낸다.
7. example.com의 IP 주소가 네임 서버에서 resolver에게 반환된다
8. DNS resolver가 처음 요청한 도메인의 IP 주소의 브라우저에 응답한다.



DNS 조회(lookup)의 8단계가 실행된 후 IP 주소가 반환되면 브라우저가 웹 페이지를 요청할 수 있다.



9. 브라우저가 IP 주소로 HTTP 요청을 보낸다.
10. IP 주소의 서버가 웹 페이지에서 렌더링할 웹 페이지를 반환한다.

<br>

<br>

------

**Reference**

- [Beginner’s Guide: What is a Domain Name and How Do Domains Work?](https://www.wpbeginner.com/beginners-guide/beginners-guide-what-is-a-domain-name-and-how-do-domains-work/)
- [Domain name](https://en.wikipedia.org/wiki/Domain_name)
- [What is DNS? | How DNS works](https://www.cloudflare.com/learning/dns/what-is-dns)

