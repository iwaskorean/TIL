# What is Semaphore?

세마포어(semaphore)란 쓰레드간 공유되는 변수이며 음이 아닌 정수 값을 가지는 변수이다. 세마포어는 공유 리소스의 갯수를 나타내며 이를 사용해 리소스에 대한 액세스를 허용하거나 차단할 수 있다.

세마포어는 시그널링 메커니즘을 따른다. 시그널링 메커니즘은 세마포어를 기다리는 쓰레드가 다른 쓰레드에 의해 시그널링될 수 있는 메커니즘이다.

세마포어는 프로세스 동기화를 위해 wait과 signal 두 가지 원자적(atomic) 연산을 사용한다. 프로세스 동기화란 공유 리소스의 일관성을 유지하기 위한 작업을 의미한다. 

<br>

### Charasteristic of Semaphore

세마포어의 특징은 다음과 같다.

- 작업 동기화를 위해 사용될 수 있는 메커니즘이다.
- 저수준 동기화 메커니즘이다.
- 세마포어는 항상 음이 아닌 정수 값을 가진다.
- 세마포어는 파일 디스크립터로 실행되는 테스트 작업과 인터럽트를 사용해 구현될 수 있다.

<br>

## Types of Semaphores

세마포어는 계수형(counting) 세마포어, 이진 세마포어 두 종류가 있다.

<br>

### Counting Semaphore

![](https://cdn.guru99.com/images/1/120319_0858_WhatisSemap1.png)

계수형 세마포어는 자원이 한정되었을 때 리소스 액세스를 제어하기 위해 계수를 사용한다. 초기 계수는 사용 가능한 리소스의 갯수로 초기화되며 초기 계수가 0인 경우 사용할 수 없는 상태(unavailable)에서 계수형 세마포어를 생성해야한다. 그러나 계수가 0보다 크면 사용 가능한 상태(avilable)에서 세마포어가 생성된다.

예를 들어 서버에 프린터가 3대 연결되어 있을때 사용자가 프린터를 사용하기 위해 서버에 요청한다고 가정하면 공유 리소스인 프린터는 3대이므로 세마포어는 3으로 초기화되고 사용자가 프린터를 사용할 때마다 1이 감소된다. 세마포어가 0인 경우 사용자는 프린터를 사용할 수 없으며 프린터 사용이 끝나고 프린터가 반환되면 세마포어는 다시 1이 증가하게된다.

<br>

### Bianry Semaphore

![](https://cdn.guru99.com/images/1/120319_0858_WhatisSemap2.png)

이진 세마포어는 계수형 세마포어와 매우 유사하지만 값이 0과 1로 제한된다. 이진 세마포어에서의 wait 연산은 세마포어가 1일때만 작동하며 signal 연산은 세마포어가 0인 경우에만 성공적으로 동작한다.

계수형 세마포어에는 상호 배제가 없지만 이진 세마포어에는 상호 배제가 있다.

<br>

## Wait and Signal Operations

wait 연산과 signal 연산은 프로세스 동기화, 즉 프로세스가 공유 리소스를 사용할 때 다른 프로세스의 액세스를 막는 상호 배제(mutual exclusion)를 위해 사용된다.

<br>

### Wait for Operation

wait 연산은 임계 영역(동시에 둘 이상의 스레드가 접근해서는 안되는 공유 리소스에 액세스하는 코드의 일부, wikipedia)내의 작업을 제어하기 위한 연산이며 세마포어 값을 1 감소시킨다. P(S) 연산이라고도 부르며 세마포어 값이 감소되어 음수가 되면 필요한 조건을 충족할 때 까지 명령이 유지된다. 

<br>

### Signal for Operation

signal 연산은 임계 영역에서 작업의 종료를 제어하기 위해 사용된다. signal 연산은 세마포어의 값을 1 증가시키며 V(S) 연산 이라고도 부른다. 

<br>

###### Todo : advantage/disadvantage of semaphore, implementing wait and signal operation, mutex

<br>

<br>

------

##### Reference

- [What is Operating System? Explain Types of OS, Features and Examples](https://www.guru99.com/operating-system-tutorial.html)
- [세마포어(semaphore) 완전 쉬운 이해! wait(), signal()](https://jhnyang.tistory.com/101)

