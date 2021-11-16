# System Call

시스템 콜이란 프로세스와 운영체제간의 인터페이스를 제공하는 매커니즘 즉, 프로세스가 하드웨어에 접근해 기능을 사용할 수 있게 해주는 매커니즘이다.

시스템 콜은 API를 통해 사용자 프로그램에 운영체제의 서비스를 제공하며 커널 시스템의 유일한 엔트리 포인트이다.

<br>

### How System Call works

운영체제의 시스템 콜 과정은 다음과 같다.

![](https://cdn.guru99.com/images/1/121119_0451_SystemCalli3.png)

- 1단계 : 위 다이어그램과 같이 시스템 콜이 인터럽트할 때까지 프로세스는 사용자 모드에서 실행된다.
- 2단계 : 커널 모드에서 시스템 콜이 실행된다.
- 3단계 : 시스템 콜이 끝나면 사용자 모드로 제어된다.
- 4단계 : 커널 모드에서 사용자 프로세스의 실행이 재개된다.

<br>

### Why do you need System Call in OS?

운영체제는 다음과 같은 상황에서 시스템 콜을 필요로한다.

- 파일을 읽기 및 쓰기
- 파일 시스템 파일 생성 및 삭제
- 새로운 프로세스 생성 및 관리
- 네트워크 패킷 전송 및 수신 
- 스캐너 ,프린터 등의 하드웨어 장치 액세스

<br>

## Types of System Calls

![](https://cdn.guru99.com/images/1/121119_0451_SystemCalli4.png)

운영체제에서 시스템 콜의 유형은 다음과 같다.

- 프로세스 제어(process control)
- 파일 관리(file management)
- 장치 관리(device management)
- 정보 유지(information maintenance)
- 통신(communication)

<br>

### Process Control

프로세스 호출 시스템 콜은 프로세스 생성, 프로세스 종료 등의 작업을 수행한다.

- 종료 및 중단(abort)
- 불러오기 및 실행
- 프로세스 생성 및 종료
- wait(), signal()
- 메모리 할당 및 해제

<br>

### File Management

파일 관리 시스템 호출은 파일 생성, 읽기 등의 파일 조작과 관련한 작업을 처리한다.

- 파일 생성
- 파일 삭제
- 파일 열기 및 닫기
- 읽기, 쓰기, 재배치(reposition)
- 파일 속성 가져오기 및 설정

<br>

### Device Management

장치 관리는 장치 버퍼에서 읽기 작업, 장치 버퍼에 쓰기 작업 등 장치 조작 작업을 수행한다.

- 장치 요청 및 해제(release)
- 장치 연결 및 분리
- 장치 속성 가져오기 및 설정

<br>

### Information Maintenance

정보 유지는 사용자 프로그램 간의 정보와 정보 전송을 처리한다.

- 시간 및 날짜 설정
- 프로세스 및 장치 속성 가져오기

<br>

### Communication

통신 시스템 콜은 프로세스간의 통신에서 사용된다.

- 통시 연결, 생성 및 삭제
- 메시지 전송 및 수신
- 운영체제가 상태 정보를 전송하는데 도움을 준다.
- 원격 장치 연결 및 분리

<br>

## Parameter for System Call

시스템 콜에 매개변수를 전달하는 규칙은 다음과 같다.

- 매개변수는 운영체제에 의해 스택에 push 또는 pop 되어야 한다.
- 매개변수는 레지스터로 전달될 수 있다.
- 레지스터보다 많은 수의 매개변수가 있는 경우, 블록에 저장하고 블록 주소를 매개변수로 레지스터에 전달해야한다.

<br>

## Important System Calls Used in OS

### wait()

상위 프로세스가 하위 프로세스를 생성하면 상위 프로세스는 wait() 시스템 콜과 함께 자동으로 실행이 중지된다. 자식 프로세스의 실행이 종료되면 부모 프로세스로 제어권이 이동한다.

<br>

### fork()

 fork() 시스템 콜은 메모리를 할당하고, 자신을 복사해 새로운 프로세스를 생성할 때 사용된다.

<br>

### exec()

exec() 시스템 콜은 메모리를 할당 없이 원래의 프로세스를 새로운 프로세스로 대체해 생성할 때 사용된다.

<br>

### kill()

kill() 시스템 콜은 운영체제에서 프로세스에 종료 신호를 보내는 데 사용된다. 특수한 경우에서는 프로세스 종료가 아닌 다른 작업을 수행하기 위해 사용될 수 있다.

<br>

### exit()

exit() 시스템 콜은 프로그램 실행을 종료하는데 사용된다.

<br>

<br>

------

##### Reference

- [What is System Call in Operating System?](https://www.guru99.com/system-call-operating-system.html)

