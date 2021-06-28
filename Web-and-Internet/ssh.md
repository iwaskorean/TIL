# SSH(Secure Shell)

SSH는 컴퓨터가 다른 컴퓨터로의 보안 원격 로그인 및 파일 전송(secure remote login and file transfer)을 가능하도록 하는 프로토콜이다.

인증을 위한 옵션들과 암호화로 보안성을 강화해 암호화를 사용하지 않는 로그인 프로토콜(telnet, rlogin)과 보안성이 떨어지는 파일 전송 방식(FTP)을 대체할 수 있다.

<br>

### How does the SSH protocol work?

![i](https://www.ssh.com/hubfs/Imported_Blog_Media/How_does_the_SSH_protocol_work_-2.png)



SSH는 SSH 서버에 연결하는 클라이언트에 의해 연결이 만들어지는 클라이언트-서버 모델이다.

SSH 클라이언트는 연결 설정 프로세스를 실행하며 public key 암호화를 사용해 SSH 서버의 ID를 확인한다. 연결 설정 단계 후에 SSH 프로토콜은 강력한 암호화와 해싱 알고리즘을 통해 클라이언트와 서버간의 교환되는 데이터의 보호와 무결성(integrity)을 보장한다.

<br>

## Understanding Different Encryption Techniques

SSH에서는 대칭 암호화(symmetrical encryption), 비대칭 암호화(asymmetrical encryption), 해싱(hashing) 총 3가지의 암호화 기술이 사용된다.

<br>

### Symmetric Encryption

![i](https://www.hostinger.com/tutorials/wp-content/uploads/sites/2/2017/07/symmetric-encryption-ssh-tutorial.jpg)

대칭 암호화는 클라이언트와 서버에서 메시지를 암호화하고 복호화하기 위해 secret key를 사용하는 방식이다. 클라이언트와 서버가 키 교환 알고리즘(key exchange algorithm)을 통해 하나의 대칭 키를 만들어 공유하며 이 대칭 키를 이용해 클라이언트와 서버가 전체 통신을 암호화한다. 

<br>

### Asymmetric Encryption

![ㅑ](https://www.hostinger.com/tutorials/wp-content/uploads/sites/2/2017/07/asymmetric-encryption.jpg)

비대칭 암호화는 대칭 암호화와 다르게 암호화와 복호화를 위해 public key와 private key 두 개의 키를 사용하며 이 두 개의 키는 public-private key 쌍을 생성한다. 이름 그대로 public key는 공개적인 키이며 private key는 비공개로 유지되는 키이다.

비대칭 암호화는 전체 통신을 암호화하는데 사용되지 않는다. 대신, 대칭 암호화의 대칭 키 생성을 위한 키 교환 알고리즘에 사용된다. 연결을 시작하기 전 클라이언트와 서버는 임시 public-private key 쌍을 생성하고 해당 private key를 공유해 공유 private key를 생성한다.

대칭 통신이 연결되면 서버는 클라이언트 public key를 사용해 클라이언트의 인증을 위한 시도를 클라이언트에 보낸다. 클라이언트가 성공적으로 복호화 한다면 해당 public key 쌍에 맞는 private key를  가지고 있는 것이므로 인증이 완료되고 SSH 세션이 시작된다.

<br>

### Hashing

![i](https://www.hostinger.com/tutorials/wp-content/uploads/sites/2/2017/07/ssh-tutorial-hash.jpg)

입력에서 암호화 해시를 생성할 수는 있지만, 해시에서 입력을 생성하는 것은 불가하다 즉, 단방향 해시는 복호화가 불가능하다. 

SSH는 해시를 사용해 입력에 대한 암호화 해시를 비교해 클라이언트가  올바른 입력을 가지고 있는가를 확인한다.

<br>

<br>

------

**Reference**

- [SSH Protocol – Secure Remote Login and File Transfer](https://www.ssh.com/academy/ssh/protocol)
- [How Does SSH Work](https://www.hostinger.com/tutorials/ssh-tutorial-how-does-ssh-work)
