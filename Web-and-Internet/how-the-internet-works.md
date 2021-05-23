# How the internet works

인터넷은 웹을 가능하게하는 중추적인 기술이다. 기본적으로 인터넷은 컴퓨터들이 통신 가능한 네트워크를 의미한다.



### A simple network

컴퓨터 2대가 통신이 필요할 때, 이더넷 케이블과 같은 물리적인 장비나 와이파이와 같은 무선으로 연결되어야 한다. 

![img](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work/internet-schema-1.png)



이러한 네트워크는 두 대의 컴퓨터에만 제한되지 않는다. 복잡하지만 원하는 만큼 컴퓨터들을 연결할 수 있다. 예를 들어 10대의 컴퓨터를 연결하기 위해서 1대당 9개의 플러그가 있는 45개의 케이블이 필요하다.

이 문제를 해결하기위해 네트워크의 각 컴퓨터들은 라우터라는 특수한 작은 컴퓨터에 연결된다. 라우터는 컴퓨터에서 보낸 메시지가 다른 컴퓨터로 정확히 도착하는지  확인하는 역할을 한다. 예를 들어 컴퓨터 B에 메시지를 보내려면 컴퓨터 A가 라우터로 메시지를 보내야하고 그러면 컴퓨터 B에 메시지가 전달되고 컴퓨터 C에는 메시지가 전달되지 않는다.  

라우터를 시스템에 추가하면 10대의 컴퓨터에 라우터를 연결하기 위한 10개의 케이블만 필요하다.

![I](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work/internet-schema-3.png)



<br>

### A network of networks

단일 라우터를 추가한 시스템으로 다수의 컴퓨터를 연결하는 네트워크를 구성할 수 있지만 수천수만, 수십억대의 컴퓨터를 연결하는데는 어려움이 있다. 그러나 컴퓨터를 라우터에 연결하고 다른 라우터를 라우터에 연결함으로써 네트워크를 무한히 확장할 수 있다.

![i](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work/internet-schema-5.png)



아주 먼 곳 다른 지역의 케이블과의 연결을 위해서는 모뎀이라는 특수장비가 필요하다. 모뎀을 사용해 이미 설치되어있는 전화 시설과 네트워크를 연결해 통신할 수 있다.

모뎀을 통해  ISP(인터넷 서비스 제공업체)에 연결되면 ISP는 일부 특수 라우터를 관리하고 다른 ISP의 라우터에도 액세스할 수 있기 때문에 네트워크 메시지가 ISP 네트워크의 네트워크를 통해서 대상 네트워크로 전달된다.

인터넷은 이와 같은 전체 네트워크 인프라로 구성된다.

![I](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work/internet-schema-7.png)

<br>

### Finding computers

컴퓨터로 메시지를 보내기위해서는 대상을 지정해야하기 때문에 네트워크에 연결된 모든 컴퓨터들은 4개의 숫자가 점으로 분리된 고유한 주소인 IP 주소를 가지고 있다.

사람이 숫자와 점으로 구성된 IP 주소를 기억하기 어렵기 때문에 도메인 이름(domain name)을 통해 IP 주소의 이름을 지정할 수 있다. 예를 들어 `google.com`은  IP 주소로 `173.194.121.32`이다. 이러한 도메인 이름을 통해 인터넷을 더 쉽게 사용할 수 있다.

<br>

### Intranets and Extranets

인트라넷은 특정 조직의 멤버들로 구성된 private한 네트워크이다. 일반적으로 멤버들이 공유 리소스에 안전하게 액세스하고 협업 할 수 있는 포탈을 제공한다. 조직의 주요 문서 및 파일 관리를 위한 공유 드라이브, 비즈니스 문서 관리 등을 위한 페이지나 도구들을 호스팅할 수 있다.

엑스트라넷은 인트라넷과 매우 유사하지만 다른 조직과의 협업을 위해 private한 네트워크의 일부를 개방한다.  비즈니스에서 기업관련자나 고객 정보를 안전하게 공유하기 위해 사용된다.

인트라넷과 엑스트라넷은 인터넷과 동일한 인프라에서 실행되며 같은 프로토콜을 사용한다.

![i](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work/internet-schema-8.png)

<br>

### Internet and the Web

일반적으로 웹 브라우저로 웹을 탐색할 때 도메인을 통해 웹사이트에 접속한다. 그러나 이것이 인터넷과 웹이 같다는 것을 의미하는것은 아니다. 인터넷은 수십억대의 컴퓨터를 연결할 수 있는 네트워크 기술 인프라이며 웹은 이러한 인프라 위에 구축되어 있는 서비스이다.

<br>

<br>

------

**Reference**

- [How does the Internet work?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work)
