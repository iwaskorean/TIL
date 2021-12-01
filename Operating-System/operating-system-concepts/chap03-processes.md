# Processes

## Process Concept

프로세스는 메모리에 로드되어 실행중인 프로그램이자 운영체제의 관점에서의 작업 단위이다.

하나의 프로세스가 실행되기 위해서는 다음과 같은 자원이 필요하다.

- CPU
- 메모리
- 파일
- 리소스(입출력 장치 등)

프로세스는 텍스트, 데이터, 힙, 스택 섹션으로 나누어진 메모리 레이아웃을 가진다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_01_Process_Memory.jpg)

- 텍스트: 실행 코드
- 데이터: 전역 변수들
- 힙: 프로그램이 실행되는 동안 동적으로 할당된 메모리
- 스택: 함수가 호출될 때의 임시 저장 공간

### Process State

프로세스가 실행되면 다음과 같은 상태 중 하나의 상태를 가진다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_02_ProcessState.jpg)

- new: 프로세스가 생성되는 단계에 있는 상태이다.
- running: CPU를 점유해서 프로세스의 명령어가 CPU에서 실행되고 있는 상태이다. running 상태의 프로세스가 time out이 되면 CPU 스케줄러는 ready 상태의 프로세스 하나
  를 dispatch한다.
- waiting: 일부 리소스나 이벤트가 발생되기를 기다리고 있기 때문에 프로세스를 실행할 수 없는 상태이다. 입출력 입력, 디스크 액세스 요청, 타이머 등 자식 프로세스가 완료되기를 기다리고 있다.
- ready: 프로세스 실행을 위한 리소스를 확보해놓은 상태지만 CPU가 현재 프로세스의 명령을 실행하고 있는 것은 아니다.
- terminated: 프로세스가 완료된 상태이다.

### PCB(process control block)

PCB란 프로세스가 가진 저장해놓은 구조체이며 프로세스를 관리하는 데 사용된다. 운영체제는 PCB에 프로세스가 가져야할 모든 정보를 저장해서 핸들링한다.

PCB에 저장되는 정보들은 다음과 같다.

- 프로세스 상태
- 프로그램 카운터: 프로세스가 실행해야할 명령의 주소를 저장
- cpu 레지스터
- 메모리 관리 정보
- 계정 정보
- 입출력 상태 정보

### Thread

기본적으로 프로세스는 한 가닥(thread)에서 실행되는 프로그램이다. 하나의 쓰레드 제어는 한번에 하나의 태스크밖에 처리할 수 없다.

쓰레드는 4장에서 자세히 나올 예정이다.

<br>

## Process Scheduling

멀티프로그래밍의 목적은 CPU 사용률을 최대화하는 것이고 시분할의 목적은 CPU 코어를 프로세스간의 전환을 통해 사용자에게 동시에 프로그램이 실행되고 있는 것처럼 보이게하는 것이다.

이 두 목적을 충족시키기위해 프로세스 스케줄러는 CPU에 적절한 프로세스를 할당하는 프로세스 스케줄링을 수행한다.

### Scheduling Queues

스케쥴링 큐는 시분할을 위한 자료구조이다. 모든 프로세스들은 job queue에 저장된다. ready 상태의 프로세스들은 ready queue로 이동되며 입출력 장치를 기다리고 있는 프로세스들은 device queue로 이동한다.

프로세스 스케줄링을 다이어그램으로 나타내면 다음과 같다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_06_QueueingDiagram.jpg)

### Context Switch

컨텍스트란 프로세스가 프로세서에 의해 사용되고 있는 상태이자 운영체제의 입장에서의 PCB라고 설명할 수 있다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_04_ProcessSwitch.jpg)

컨텍스트 스위치란 인터럽트가 발생하면 현재 running 중인 프로세스의 컨텍스트를 저장하고 다시 CPU를 획득했을 때 저장해놓은 컨텍스트를 리스토어하는 것이다.

시분할, 멀티프로세싱은 운영체제에 의한 컨텍스트 스위치를 통해 수행된다.

<br>

## Operations on Processes

프로세스는 새로운 프로세스를 생성할 수 있다.

### Process Creation

fork() 시스템 콜을 통해 프로세스를 생성할 수 있다. fork() 시스템 콜로 새로운 프로세스를 생성하면 항상 new 상태가 된다. 프로세스를 만드는 상위 프로세스를 부모 프로세스, 만들어지는 하위 프로세스를 자식 프로세스라고 한다.

시스템이 시작되면 init 프로세스가 실행된다. 이 프로세스의 pid는 1이며 생성되는 프로세스들의 트리 구조에서 init 프로세스는 루트 노드 프로세스가 된다.

부모 프로세스가 자식 프로세스를 생성한 후 다음과 같은 두 가지 옵션으로 실행될 수 있다.

- 자식 프로세스가 실행되고 자식 프로세스가 terminated 될 때까지 wait() 시스템 콜을 통해 부모 프로세스가 자식 프로세스의 종료를 기다린다.
- 자식 프로세스를 생성한 후 두 프로세스가 동시에 실행된다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_10_ProcessCreation.jpg)

자식 프로세스의 주소 공간은 다음과 같은 두 가지 옵션으로 실행될 수 있다.

- 자식 프로세스가 부모와 같은 프로그램에 동일한 동작을 하는 프로세스이면 PCB만 다르고 같은 주소 공간에서 실행된다.
- 다른 주소 공간에서 로드된 새로운 프로그램을 가질 수 있다.

### Process Termination

프로세스는 다음과 같은 상황에서 종료된다.

- 마지막 statement의 실행이 종료될 때
- exit() 시스템 콜을 통한 강제 종료
- 운영체제가 시스템에 필요한 리소스를 제공할 수 없을 때

부모 프로세스가 fork() 시스템 콜을 통해 자식 프로세스를 생성한 즉시 부모 프로세스의 실행이 wait() 시스템 콜 없이 termintated되면 자식 프로세스만 남게되고 이 자식 프로세스를 좀비 프로세스라고 부른다.

<br>

## Example

```c
// Q1. LINE X 라고 표기된 위치의 실행 순서는 ?

int main() {
  pid_t pid = fork();

  if (pid > 0) {
    wait(NULL);
    // LINE A
  } else {
    pid = fork();

    if (pid == 0) {
    // LINE B
    } else {
    wait(NULL);
    // LINE C
    }
  }
  // LINE D
}

// B - D - C - D - A - D
```

```c
// Q2. 코드 실행 결과는 ?

int x = 10;

int main() {
  pid_t pid = fork();

  if (pid == 0) {
    x += 10;
  }
  else {
    wait(NULL);
    pid = fork();
    x += 10;

    if (pid > 0) {
      wait(NULL);
    } else {
      x += 10;
    }
  }

  printf("%d ", x);

}

// 20 30 20
```

###### Note: 3.4 ~

---

### Reference

- [운영체제 공룡책 강의](https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum)
