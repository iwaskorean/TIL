# User Mode vs Kernel Mode

운영체제를 실행하는 프로세서에는 사용자 모드와 커널 모드 두 가지 모드가 있다.

프로세서는 프로세스의 종류에 따라 다른 모드를 사용한다. 사용자 모드와 커널 모드의 구분을 위해 CPU 내부의 mode 비트를 사용한다. 응용 프로그램은 사용자 모드에서 사용되고, 핵심 운영체제 구성요소들은 커널 모드에서 실행된다.

대부분의 장치 드라이버들은 커널 모드에서 실행되지만, 사용자 모드에서도 일부 드라이버들이 실행될 수 있다.

<br>

### User Mode

사용자 모드 응용 프로그램을 시작하면 운영체제에서 응용 프로그램에 대한 프로세스를 생성한다. 생성된 프로세스는 응용 프로그램에 대한 가상 주소 공간(virtual address space)과 private handle table을 제공한다. 각 응용 프로그램은 개별적으로 실행된다.

사용자 모드 응용 프로그램의 가상 주소 공간은 private하며 제한되어 있다. 따라서 사용자 모드에서 실행중인 프로세서는 운영체제의 사용을 위해 예약된 가상주소에 접근 할 수 없다. 또한 가상 주소 공간의 제한을 통해 데이터 변경 및 손상을 방지할 수 있다.

사용자 모드에서 mode 비트는 1로 설정되며 커널 모드로 전환될 때 mode 비트가 1에서 0으로 변경된다.

<br>

### Kernel Mode

시스템은 부팅할 때 커널 모드에서 시작되고 운영체제의 로드가 완료되면 사용자 모드에서 응용 프로그램을 실행한다. 

인터럽트 명령어, 입출력 관리 등 커널 모드에서만 실행 가능한 특권 명령(privileged instructions)이 있다. 이러한 명령어들은 사용자 모드에서 실행되선 안되며 실행될 경우 트랩이 생성된다. 

![](https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/userandkernelmode01.png)

커널 모드에서 실행되는 모든 코드들은 다른 드라이버 및 운영체제와 독립적으로 가상 주소 공간을 가지고 있는 것이 아니라 단일 주소 공간을 공유한다. 따라서 커널 모드 드라이버가 충돌하면 전체 운영체제가 충돌한다.

커널 모드에서 mode 비트는 0으로 설정되며 커널 모드에서 사용자 모드로 전환할 때 0에서 1로 변경된다.

<br>

### Necessity of User Mode and Kernel Mode in Operating System

운영체제에서 사용자 모드와 커널 모드, 이중 모드의 전환이 없을 경우 다음과 같은 문제가 발생할 수 있다.

- 사용자 프로그램이 사용자 데이터로 운영체제 데이터를 덮어써 운영체제가 손상될 수 있다.
- 다수의 프로세스가 동시에 같은 시스템을 사용해 문제가 발생한다.

<br><br>

------

##### Reference

- [User Mode vs Kernel Mode](https://www.tutorialspoint.com/User-Mode-vs-Kernel-Mode)
- [User mode and kernel mode](https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)

