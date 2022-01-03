# Synchronization Tools

## Background

협력적(cooperating) 프로세스들은 다른 프로세스 실행에 영향을 주거나 다른 프로세스로부터 영향을 받을 수 있으며 논리 주소 공간과 공유 메모리를 통해 데이터를 공유할 수 있다.

그러나 위와 같이 동시성을 가지는 프로세스들이 공유 데이터에 액세스하면 데이터 불일치(data inconsistency) 문제를 야기할 수 있다.

따라서 우리는 cooperating 프로세스들의 순서있는 실행(orderly execution)을 통해 데이터 일관성을 유지해야 한다.

### Producer-Consumer Problem

생산자와 소비자 문제를 통해 동시성을 가지는 프로세스들의 데이터 불일치 문제를 생각해보자.

생산자는 버퍼에 새 아이템을 추가할 때마다 count 변수를 1 증가시키고 소비자는 아이템을 꺼낼 때마다 count 변수를 1 감소시킨다.

```cpp
while(true){
	while(count == BUFFER_SIZE) // do nothing

  buffer[in] = next_produced;
  in = (in + 1) % BUFFER_SIZE;
  count++;
}
```

```cpp
while(true){
	while(count == 0) // do nothing

  next_consumed = buffer[out];
  out = (out + 1) % BUFFER_SIZE;
  count--;
}
```

count 변수를 계산하는 코드를 기계어 수준으로 보면 레지스터에 값을 할당해 count 값을 갱신하는 방식이다.

```
// count 증가
register1 = count
register1 = register1 + 1
count = register1

// count 감소
register2 = count
register2 = register2 - 1
count = register2
```

count++과 count--를 동시에 실행하면 다음과 같이 교차되어 실행될 수 있다.

```
T0: 생산자가 register1 = count를 실행 {register1 = 5}
T1: 생산자가 register1 = register1 + 1를 실행 {register1 = 6}
T2: 소비자가 register2 = count를 실행 {register2 = 5}
T3: 소비자가 register2 = register2 - 1를 실행 {register2 = 4}
T4: 생산자가 count = register1를 실행 {count = 6}
T5: 생산자가 count = register2를 실행 {count = 4}
```

위와 같이 실행될 경우 정상적인 count의 값은 5이지만 실제 실행 결과는 4로 계산되었다.

그 이유는 동시성을 가진 프로세스가 공유 데이터인 count를 조작할 때 액세스하는 순서에 따라 결과가 달라지기 때문이다.

이를 경쟁 상태(race condition)라고 한다.

- 여러 개의 프로세스, 쓰레드가 데이터를 공유하고 있고 동시성을 가지고 처리하려고 할 떄 발생한다.
- 실행 결과는 공유 데이터를 언제 액세스 하느냐에 따라 결과가 달라진다.
- 경쟁 상태를 막기 위해서는 프로세스 동기화를 통해 하나의 프로세스만 공유 데이터에 액세스할 수 있도록 보장해야 한다.

<br>

## Critical Section

n개의 프로세스로 구성된 시스템이 있을 때 각 프로세스는 공유 데이터에 접근하고 갱신할 수 있는 코드 영역인 임계 구역(critical section)을 포함하고 있다.

하나의 프로세스가 임계 구역을 실행하고 있으면 다른 프로세스들은 임계 구역으로 접근할 수 없다.

이와 같이 두 개 이상의 프로세스가 동시에 임계 구역에서 실행될 수 없게하면 자연스럽게 프로세스를 동기화 할 수 있다.

임계 구역은 다음과 같은 요구 사항을 충족해야 한다.

- 상호 배제(mutual exclusion): 한 프로세스가 임계 구역에서 실행 중일 때 다른 프로세스는 함께 실행될 수 없다.
- 진행(progress): 임계 구역밖에서 실행중인 프로세스들만 임계 구역으로 진입하기 위한 프로세스들을 결정할 수 있다.
- 한정 대기(bounded waiting): 대기의 제한을 두어서 임계 구역 진입에 한정적으로 대기하도록 해야한다.

일반적으로 운영체제는 임계 구역을 다음과 같은 두 가지 방법으로 처리한다.

- 비선점형 커널
  - 한 프로세스가 커널 모드를 종료할 때까지 계속해서 선점해서 경쟁 상태를 회피한다.
  - 속도가 느려 잘 사용하지 않는다.
- 선점형 커널
  - 우선순위가 높은 다른 프로세스가 언제든지 선점할 수 있다.
  - 설계에 어려움이 있지만 더 반응적이다.

<br>

## Petereson's Solution

임계 영역 문제를 해결하기 위한 다양한 솔루션들이 있다.

- 데커 알고리즘
- 베이커리 알고리즘
- 엘리슨버그와 맥과이어 알고리즘
- 피터슨 알고리즘

이 중 피터슨 알고리즘, 피터슨 솔루션에 대해 알아보자.

- 임계 구역과 다른 구역을 번갈아가며 실행되는 두 개의 프로세스로 한정된다.
- 임계 구역에 진입할 순서를 나타내는 turn 변수와 임계 구역에 진입할 준비를 나타내는 flag 변수를 사용한 알고리즘이다.

```cpp
int turn;
boolean flag[2];

while(true){
	flag[i] = true;
    turn = j;
    while(flag[j] && turn == j)

    // critical section

    flag[i] = false;

    //remainder section
}
```

피터슨 솔루션은 임계 구역 문제 해결을 위한 개념적 솔루션이다.

- 제대로 동작한다고 보장되지 않는다.(no guarantee)
- flag와 turn의 값이 변경될 때 컨텍스트 스위치가 발생하면 결과 값이 올바르지 않을 수 있다.
- 그러나 알고리즘, 개념적으로는 완전히 증명 가능하다.
- 상호 배제 조건을 충족하며 데드락과 기아 현상이 발생하지 않는다.

<br>

## Hardware Based Solution

임계 구역 문제 해결을 위한 하드웨어 명령어는 다음과 같다.

- 메모리 장벽: 메모리의 변경 사항을 다른 프로세스들에게 전달해 다른 프로세서에서 실행중인 쓰레드에 메모리 변경 사항을 보이게하는 명령어다.
- 하드웨어 명령어

  - test_and_set()

    ```
    boolean test_and_set(boolean *target){
      boolean rv = *target;
      *target = true;
      return rv;
    }
    do{
      while(test_and_set(&lock))
        //do nothing

        //critical section

        lock = false;

        //remainder section

    }while(true);
    ```

  - compare_and_swap()

    ```
    while(true){
      waiting[i] = true;
      key = 1;
      while(waiting[i] && key == 1){
        key = compare_and_swap(&lock, 0, 1);
      }
      waiting[i] = false;

      j = (i + 1) % n;
      while((j != i) && !waiting[j]){
        j = (j + 1) % n;
      }

      if(j == i) lock = 0;
      else waiting[j] = false;
    }
    ```

<br>

## Mutext Locks

위에서 살펴본 피터슨 알고리즘과 하드웨어 명령어들은 실제 개발자가 사용하기에는 어려움이 있다.

이를 대신해 사용할 수 있는 고수준 소프트웨어 도구들이 있는데 이 중 뮤텍스 락은 피터슨 알고리즘과 하드웨어 명령어 대신 임계 구역 문제를 해결할 수 있는 가장 간단한 동기화 도구이다.

임계 구역에 들어갈 때 열쇠, 락을 받고 들어간 후 나올 때 락을 반환하는 형태이다.

```cpp
while(true) {
  // acquire lock
  ciritcial section

  // release lock
  remainder section
}
```

뮤텍스 락은 바쁜 대기(busy waiting)가 발생한다는 단점이 있다.

- 바쁜 대기란 한 프로세스가 임계 구역에 있는 동안 다른 프로세스들은 반복문을 호출해야 한다.
- 바쁜 대기를 하면서 기다리고 있는 락을 스핀락이라고 한다.
- 멀티코어에서의 스핀락은 컨텍스트 스위치를 위해 ready queue에 안들어가도 되기 떄문에 비용적인 부분에서 장점도 있다.

<br>

## Semaphores

세마포어는 사전적으로 신호장치라는 뜻이다. 정수 변수 S를 신호삼아 프로세스들을 동기화한다.

S가 1이면 뮤텍스 락과 같이 동작하며 S가 n이면 여러개의 인스턴스를 가진 자원들을 사용할 수 있다.

S를 초기화한 이후 wait(), signal() 연산을 통해 S의 값을 증감시킨다.

- 자원을 사용할 때 wait() 연산을 통해 S를 감소 시킨다.
- 자원을 릴리즈할 때 signal() 연산을 통해 S를 증가시킨다.
- S가 0이 되면 모든 자원이 사 용중인 것이므로 블락된다.

```cpp
wait(S) {
  while(S<=0)
  ; // busy wait
  S--;
}

signla(S) {
  S++;
}
```

<br>

## Monitor

뮤텍스와 세마포어는 효과적인 동기화 도구이지만 다음과 같은 경우들에서 타이밍 에러가 발생할 수 있다.

- wait()와 signal() 연산의 순서가 바뀌어 수행될 때
- wait() 연산 다음 다시 wait() 연산이 수행될 때

타이밍 에러가 발생하면 두 개 이상의 프로세스가 임계 구역에 진입해 문제가 발생한다.

이러한 문제점을 해결하기 위한 것이 뮤텍스와 세마포어보다 더 심플한 동기화 도구인 모니터이다.

### Monitor Usage

![monitor](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_16_MonitorSchematic.jpg)

모니터 타입은 상호배제를 제공해주는 ADT(abstarct data type)이다. 쉽게 말해 모니터 타입은 데이터와 데이터를 조작하는 연산자들을 하나의 단위로 추상화한 데이터 타입이다.

모니터는 공유 데이터, 초기화 코드, 공유 데이터 연산자로 구성되어 있으며 모니터 내부에 지역적으로 선언된 변수들과 매개변수들에는 모니터 내부에 정의된 연산자로만 접근할 수 있다.

모니터는 condition이라는 변수를 통해 동기화를 제공한다. condition 변수를 통해 wait(), signal() 연산을 수행할 수 있다.

```
condition x, y;

x.wait();
x.signal();
```

- x.wait(): 호출한 프로세스는 다른 프로세스가 signal() 함수를 호출할 때 까지 차단되고 차단된 프로세스는 queue에서 대기한다.
- x.signal(): 대기하고 있던 프로세스를 깨운다.

###### NOTE: 더 추가해서 정리해야할 듯 ..
