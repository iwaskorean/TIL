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
- 프로그레스(progress): 임계 구역밖에서 실행중인 프로세스들만 임계 구역으로 진입하기 위한 프로세스들을 결정할 수 있다.
- 한정 대기(bounded waiting): 대기의 제한을 두어서 임계 구역 진입에 한정적으로 대기하도록 해야한다.

일반적으로 운영체제는 임계 구역을 다음과 같은 두 가지 방법으로 처리한다.

- 비선점적 커널
  - 한 프로세스가 커널 모드를 종료할 때까지 계속해서 선점해서 경쟁 상태를 회피한다.
  - 속도가 느려 잘 사용하지 않는다.
- 선점적 커널
  - 다른 프로세스가 언제든지 선점할 수 있다.
  - 설계에 어려움이 있지만 더 반응적이다.
