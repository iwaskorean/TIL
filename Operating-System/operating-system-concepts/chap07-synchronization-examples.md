# Synchronization Examples

## Classic Problems of Synchronization

다음은 동시성 제어의 고전적인 문제들이다.

### Bounded-Buffer Problem

한정 버퍼, 생산자-소비자 문제는 중간 버퍼를 통해 자원을 공유하며 데이터를 주고 받을 때 발생하는 문제이다.

- n개의 버퍼가 있을 때 각각의 버퍼는 공유하기 위한 자원을 보유할 수 있다.
- 생산자는 버퍼를 자원으로 채우고, 소비자는 버퍼내의 공유 자원을 소비한다.

생산자-소비자 문제는 세마포어를 통해 문제점을 일부분 해결할 수 있다.

```cpp
// Sahred data Structures
int n;
semaphore mutex = 1; //mutex는 바이너리 세마포어이며 버퍼풀에 동시에 접근할 때 상호배제 조건을 충족하기 위해 1로 초기화한다.
semaphore empty = n; // 버퍼내의 저장할 공간을 나타낸다.
semaphore full = 0; // 버퍼내 소비할 아이템 수를 나타낸다.
```

```cpp
// Producer Process

while(true) {

  // produce an item in next_produced

  wait(empty); // 버퍼에 빈 공간이 생길 때까지 기다린다.
  wait(mutex); // 임계 구역에 진입할 수 있을 때까지 기다린다.

  // add produced to the buffer

  signal(mutex); // 임계 구역을 빠져나왔다고 알려준다.
  signal(full)  // 버퍼에 아이템이 있다고 알려준다.
}
```

```cpp
// Consumer Process

while(true) {
  wait(full); // 버퍼에 아이템이 생산될 때까지 기다린다.
  wait(mutex);

  // remove an item from buffer to next_cosumed

  signal(mutex);
  signal(empty); // 버퍼에 빈 공간이 생겼다고 알려준다.

  // cosume the item in next_cosumed
}
```

- 생산자는 empty가 될 때까지 즉, 버퍼가 비게 될 때까지 wait 해야 한다.
- 생산자가 여럿이면 임계 구역에 동시에 들어갈 수 있으므로 mutex를 체크하고 버퍼에 write한다.
- 소비자는 생산자와 반대로 full을 wait하고 생산자의 signal(full) 연산을 수행하면 버퍼의 아이템을 소비한다.
- signal과 wait 연산의 순서가 올바르지 않으면 오류가 발생한다.

### Readers-Writers Problem

독자-저자 문제는 공유 자원을 읽기만 하는 독자(reader) 프로세스와 공유 자원을 업데이트하는 저자(writer) 프로세스가 있을 때 발생하는 문제이다.

- 두 개 이상의 독자 프로세스들이 공유 자원에 접근해도 문제가 생기지 않는다.
- 그러나 다수의 저자 프로세스들이 공유 자원에 접근해 공유 자원을 업데이트할 경우 문제가 발생한다.

우선순위를 부여한 독자-저자 프로세스 형태도 존재한다.

- 저자 프로세스가 공유 자원을 업데이트하는 것이 독자 프로세스가 공유 자원에 접근해 공유 자원을 읽는 것보다 시간이 더 소요되므로 저자 프로세스에게 우선순위를 부여한다.
- 따라서 저자 프로세스가 공유 자원에 접근하기 위해 기다리고 있는 상태에서는 독자 프로세스가 공유 자원에 접근할 수 없다.
- 저자 프로세스가 너무 많게 되면 독자 프로세스가 공유 자원에 접근할 수 없게 되는 기아 상태가 발생한다.

독자-저자 문제는 세마포어를 통해 해결할 수 있다.

```cpp
semaphore rw_mutex = 1; // 독자 프로세스와 저자 프로세스를 동기화한다.
semaphore mutex = 1;
int read_count = 0; // 공유 자원에 접근중인 독자 프로세스의 수
```

```cpp
// Writer Process

while(true) {
  wait(rw_mutex); // 임계 구역에 들어가기 위해 기다린다.

  // writing is performed

  signal(rw_mutex); // 임계 구역에서 빠져나왔음을 알린다.
}
```

```cpp
// Reader Process

while(true) {
  wait(mutex);
  read_count++
  if(read_count == 1)
    wait(rw_mutex); // 현재 임계 구역에 있는 저자가 있을 경우 대기한다.
  signal(mutex);

  // reading is performed

  wait(mutex);
  read_count--;
  if(read_count == 0)
    signal(rw_mutex);
  signal(mutex);
}
```

### Dining Philosopers Problem

![dining philosopers problem](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7b/An_illustration_of_the_dining_philosophers_problem.png/440px-An_illustration_of_the_dining_philosophers_problem.png)

식사하는 철학자들 문제는 원탁에 앉은 철학자들 사이에 젓가락이 한 쪽씩 놓여 있고 양 쪽 젓가락을 모두 집어야 식사를 할 수 있다고 가정할 때 생기는 문제이다.

- 동시에 같은 젓가락 집게 되면 상호 배제 조건을 충족하지 못한다.
- 철학자들이 다른 젓가락을 한 쪽씩 집고 있으면 데드락과 기아현상이 발생한다.

세마포어를 사용하면 상호 배제 조건을 충족할 수 있다. 그러나 이 방법의 경우 데드락과 기아 상태는 여전히 해결할 수 없다.

```cpp
semaphore chopstick[5];

while(true) {
  wait(chopstick[i]);
  wait(chopstick[(i + 1) % 5]);

  // eat for a while

  signal(chopstick[i]);
  signal(chopstick[(i + 1) % 5]);

  // think for a while
}
```

데드락은 다음과 같은 방법을 해결할 수 있다.

- 철학자의 수를 4명으로 제한한다.
- 양 쪽 젓가락이 모두 놓여있을 때 만 젓가락을 집을 수 있게 한다.
- asymmetric한 방식으로 해결한다.
  - 홀수 번호의 철학자들은 왼쪽 젓가락을 먼저 집고 오른쪽 젓가락을 집는다.
  - 짝수 번호의 철학자들은 오른쪽 젓가락을 먼저 집고 왼쪽 젓가락을 집는다.

또는 동기화 도구인 모니터를 사용할 수 있다.

- 양 쪽 젓가락이 모두 있을 때만 젓가락을 집을 수 있게 강제한다.
- thinking, hungry, eating 세 가지 상태를 이용한다.
- 철학자들은 식사를 하고 있지 않을 때 상태를 eating으로 바꿀 수 있다.

```cpp
monitor DiningPhilosophers {

  enum {THINKING, HUNGRY, EATING} state[5];
  condition self[5];

  void pickup(int i) {
    state[i] = HUNGRY;
    test(i);
    if (state[i] != EATING)
      self[i].wait();
  }

  void putdown(int i) {
    state[i] = THINKING;
    test((i + 4) % 5);
    test((i + 1) % 5);
  }

  void test(int i) {
    // 본인이 HUNGRY 상태이며 양쪽 젓가락으 모두 집을 수 있으면 식사를 시작한다.
    if((state[(i + 4) % 5]) != EATING)
        && (state[i] == HUNGRY)
        && (state[(i + 1) % 5] != EATING) {
          state[i] = EATING;
          self[i].signal();
    }
  }

  intialization_code() {
    for(int i = 0; i < 5 ; i++)
      state[i] = THINKING;
  }
}
```

그러나 이 방법 또한 기아 상태는 해결할 수 없다.

---

### Reference

- [운영체제 공룡책 강의](https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum)
- [생산자-소비자 문제](https://ko.wikipedia.org/wiki/%EC%83%9D%EC%82%B0%EC%9E%90-%EC%86%8C%EB%B9%84%EC%9E%90_%EB%AC%B8%EC%A0%9C)
- [독자-저자 문제](https://ko.wikipedia.org/wiki/%EB%8F%85%EC%9E%90-%EC%A0%80%EC%9E%90_%EB%AC%B8%EC%A0%9C)
- [식사하는 철학자들 문제](https://ko.wikipedia.org/wiki/%EC%8B%9D%EC%82%AC%ED%95%98%EB%8A%94_%EC%B2%A0%ED%95%99%EC%9E%90%EB%93%A4_%EB%AC%B8%EC%A0%9C#:~:text=%EC%8B%9D%EC%82%AC%ED%95%98%EB%8A%94%20%EC%B2%A0%ED%95%99%EC%9E%90%EB%93%A4%20%EB%AC%B8%EC%A0%9C%EB%8A%94%20%EC%A0%84%EC%82%B0%ED%95%99%EC%97%90%EC%84%9C%20%EB%8F%99%EC%8B%9C%EC%84%B1%EA%B3%BC,%EC%97%90%20%ED%8F%AC%ED%81%AC%EA%B0%80%20%ED%95%98%EB%82%98%EC%94%A9%20%EC%9E%88%EB%8B%A4.)
