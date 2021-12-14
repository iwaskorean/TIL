# Thread & Concurrency

## Oveview

쓰레드는 CPU를 점유하는 단위이다. 쓰레드는 쓰레드id, 프로그램 카운터, 레지스터, 스택으로 구성되며 코드 섹션, 데이터 섹션, 파일 섹션과 운영체제 리소스를 공유한다.

프로세스가 다수의 쓰레드를 가지면 한번에 하나 이상의 태스크 즉, 동시에 여러 작업을 수행할 수 있다.

![single-and-double-threaded](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_01_ThreadDiagram.jpg)

멀티쓰레딩의 장점은 다음과 같다.

- 반응성: 유저 인터페이스를 처리할 때 blocking 없이 non-blocking으로 처리할 수 있다.
- 리소스 공유: 쓰레드들은 데이터 섹션을 공유하므로 리소스를 공유할 수 있다.
- 경제성: 코드 섹션을 복사할 필요 없이 코드 섹션으로 쓰레드를 나눠서 사용하므로 프로세스 컨텍스트 스위칭과 비교했을 때 쓰레드 스위칭이 더 경제적이다.
- 확장성: 멀티프로세서 아키텍쳐의 장점을 사용할 수 있다.

### Motivation of multithreading

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_02_MultithreadedArchitecture.jpg)

멀티쓰레딩의 대표적인 예는 웹 서버다. 쓰레드는 여러 자원을 공유하므로 프로세스보다 크기가 작다. 따라서 작업을 위한 프로세스 생성보다 쓰레드를 생성하는 것이 더 경제적이다.

멀티쓰레드를 사용하면 요청을 순차적으로 처리하거나 들어오는 요청에 대해 별도의 프로세스 분리 없이 동시성을 가지고 여러 처리를 동시에 처리할 수 있다.

<br>

## Multicore Programming

멀티쓰레딩은 멀티코어 시스템을 기반으로 동작한다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_03_ConcurrentSingleCore.jpg)

- 싱글코어에서의 동시 실행
  - 인터리빙(사이사이에 쓰레드를 끼워넣는 것)이 필요하다

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter4/4_04_ParralelMulticore.jpg)

- 멀티코어에서의 병렬 실행
  - 쓰레드가 사용 가능한 코어에 분산되어 병렬처리가 가능하다.

멀티코어는 싱글코어와 비교해 성능이 뛰어나지만 고려해야할 부분들이 많다.

- 태스크 식별(identifying): 동시에 실행될 수 있는 태스크 영역을 식별해야 한다.
- 밸런스: 태스크들이 균등하게 실행되어야 한다.
- 데이터 스플리팅: 독립적인 코어들에서 데이터들을 어떻게 분할할 것인가에 대해 고려해야 한다.
- 데이터 의존: 태스크들을 실행할 때 동기화 문제를 해결해야 한다.
- 테스트와 디버깅

멀티코어 시스템에서의 병렬 처리에는 두 가지 유형이 있다.

- 데이터 병렬 처리: 데이터를 여러 코어(쓰레드)로 나누고 다시 데이터의 서브 데이터들에도 같은 작업을 수행한다.
- 태스크 병럴 처리: 수행할 태스크를 서로 다른 코어에 분할해 동시에 수행한다.
- 현재는 이 둘을 분리할 필요 없이 분산 시스템을 통해 병렬 처리를 수행한다.

<br>

## Multithreading Models

쓰레드에는 사용자 쓰레드, 커널 쓰레드 두 종류가 있다.

- 사용자 쓰레드
  - 커널의 개입 없이 커널 위에서 동작하는 쓰레드이다.
  - 사용자 모드에서 사용하는 쓰레드이다.
- 커널 쓰레드
  - 운영체제가 직접 관리하는 쓰레드이다.
  - 커널 모드에서 사용하는 쓰레드이다.

사용자 쓰레드와 커널 쓰레드는 다음과 같은 관계 모델을 가진다.

- Many-to-One 모델: 다수의 사용자 쓰레드가 단일 커널 쓰레드에 매핑된다. 한 쓰레드가 block 시스템 콜을 하면 전체 프로세스가 멈추게되며 단일 커널 쓰레드이므로 프로세스를 여러 CPU로 분할할 수 없다.
- One-to-One 모델: Many-to-One 모델의 문제를 해결했지만 커널 쓰레드를 생성하는 오버헤드 발생시 시스템 속도 저하를 발생시킨다는 단점이 있다. Window95 ~ XP가 이 모델을 통해 구현되었다.
- Many-to-Many 모델: 사용자 쓰레드 생성에 제한이 없으며 프로세스를 여러 프로세스로 분할할 수 있다.

<br>

## Thread Libraries

쓰레드 라이브러리는 쓰레드 생성 및 관리하는 API이다.

- POSIX Pthread: POSIX 표준의 확장이며 사용자, 커널 쓰레드 모두 지원한다.
- Win32 thread: Window 시스템에서 커널 라이브러리로 제공된다.
- Java thread

<br>

## Implicit Treading

다음은 쓰레드 라이브러리를 통한 쓰레드를 생성하고 관리하는 복잡성을 해결하기 위한 쓰레딩 방법에 대한 설명이다.

- Thread pool
  - 처음 시작될 때 쓰레드를 다수 생성한 후 쓰레드 풀에 넣어놓고 필요할 때 쓰는 방식이다.
  - 풀에 사용가능한 쓰레드가 없을 경우 사용이 완료된 쓰레드가 다시 풀에 반환될 때까지 대기했다가 사용해야 한다.
- OpenMP
  - 병렬 코드를 자동으로 생성하도록 컴파일러에 지시한다.
- Grand Central Dispatch(GCD)
  - iOS, Mac에서 사용되는 쓰레딩이다.

<br>

## Summary

- 쓰레드란 프로세스의 실행 흐름 즉, 프로세스가 할당받은 자원을 사용하는 실행 흐름의 단위이다.
- 쓰레드는 CPU 입장에서 최소 작업 단위이다.
- 쓰레드는 독립적인 쓰레드id, pc, 레지스택, 스택을 가지고 있으며 코드/데이터/파일 섹션과 운영체제 리소스를 공유한다.
- 프로세스가 다수의 쓰레드를 가지면 동시에 여러 작업을 수행할 수 있다.
- 쓰레드에는 사용자 쓰레드, 커널 쓰레드 두 종류가 있다.
- 쓰레드 라이브러리를 통해 명시적으로 쓰레드를 관리할 수 있으며 Thread pool, OpenMP와 같이 암시적으로 쓰레드를 관리할 수도 있다.

---

### Reference

- [운영체제 공룡책 강의](https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum)
- [프로세스와 스레드의 차이](https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)
