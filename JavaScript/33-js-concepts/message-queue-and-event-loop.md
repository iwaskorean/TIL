# Message Queue and Event Loop

<br>

- 자바스크립트는 동시에 하나의 작업을 할 수 있는 단일스레드 기반의 언어이다.

<br>

#### JavaScript Runtime

![img](https://image.toast.com/aaaadh/real/2018/techblog/b1493856379d11e69c16a9a4cf841567.png)

<br>

#### Javscript Engine

- Call Stack : 호출되는 함수가 저장되는 스택구조의 메모리 영역
- Heap : 객체가 할당되는 구조화되지않은 메모리 영역

<br>

#### Web API 

- 브라우저에서 제공하는 DOM Events, AJAX, Timer 등의 API
- 비동기 호출을 위해 사용되는 `setTimeout`이나 `XMLHttpRequest`와 같은 함수도 Web API 안에 정의되어 있다.
- 호출 스택에서 실행된 비동기 함수는 Web API를 호출하고, Web API는 콜백 함수를 Task Queue에 넣는다.

<br>

<br>

### 비동기 요청과 동시성(Concurrency)

> *동시에 하나의 작업만을 할 수 있는 단일 스레드 기반의 언어인 자바스크립트에서 비동기 요청과 동시성에 대한 처리는 어떻게 할 것인가?*

- 자바스크립트 엔진 중 하나인 V8은 단일 호출 스택(Call stack)을 사용해 요청을 순차적으로 호출 스택에 담아 처리

- 비동기 처리 및 동시성에 대한 처리는 자바스크립트 엔진을 구동하는 환경, 즉 브라우저와 Node.js가 담당

  ​	![nodejs](https://image.toast.com/aaaadh/real/2018/techblog/Bt5ywJrIEAAKJQt.jpg)

  > Node.js : Chrome V8 JS 엔진으로 빌드된 자바스크립트 런타임, 자바스크립트를 활용해 Non-Blocking과 단일스레드 이벤트 루프를 통한 높은 처리 성능을 가진다.

  <br>

- 따라서, 자바스크립트가 단일스레드 기반의 언어라는 말은 **자바스크립트 엔진이 단일 호출 스택을 사용한다는 의미**이며 **자바스크립트가 구동되는 환경에서는 주로 여러개의 스레드가 사용**된다. 이러한 **구동환경이 단일 호출 스택을 사용하는 자바스크립트 엔진과 상호 연동하기 위해 사용하는 장치가 바로 <u>이벤트 루프</u>**인 것이다.

<br>

<br>

### 단일 호출 스택과 함수 실행 방식(Run-to-Completion)

- Run to Completion : 하나의 함수가 실행되면 이 함수의 실행이 끝날때까지 다른 어떤 작업도 중간에 끼어들지 못하는 함수 실행 방식

-  자바스크립트 엔진은 하나의 호출 스택을 사용하며, 현재 스택에 쌓여있는 모든 함수들이 실행을 마치고 스택에서 제거되기 전까지는 다른 어떠한 함수도 실행될 수 없다. 

  ```
  function delay() {
      for (var i = 0; i < 100000; i++);
  }
  function foo() {
      delay();
      bar();
      console.log('foo!'); // #3
  }
  function bar() {
      delay();
      console.log('bar!'); // #2
  }
  function baz() {
      console.log('baz!'); // #4
  }
  
  setTimeout(baz, 10); //#1
  foo();
  
  // 'bar!' → 'foo' → 'baz!
  ```

  - 위 코드의 주석 `#number`들의 시점에서의 호출 스택을 그림으로 나타내면

    ![img](https://image.toast.com/aaaadh/real/2018/techblog/46cb891a36d611e68728231d5bce2f36.png)

  - `setTimeout` 함수는 브라우저에게 타이머 이벤트를 요청한 후 바로 스택에서 제거

  - `foo` 함수가 스택에 추가되고 `foo` 함수가 내부적으로 실행하는 함수들이 차례대로 스택에 추가되었다가 제거

  - `foo` 함수가  실행을 마치면서 호출 스택이 비워진 상태가 됨

  - `baz` 함수가 스택에 추가되어 콘솔에 찍히게 됨

<br>

<br>

### Task Queue and Event Loop

- 태스크 큐(메시지 큐) : 콜백 함수들이 대기하는 큐(FIFO)의 형태의 배열

- 이벤트 루프 : 호출 스택이 비워질때마다 큐에서 콜백 함수를 꺼내와서 실행하는 역할

  ```
  function delay() {
      for (var i = 0; i < 100000; i++);
  }
  function foo() {
      delay();
      console.log('foo!');
  }
  function bar() {
      delay();
      console.log('bar!');
  }
  function baz() {
      delay();
      console.log('baz!');
  }
  
  setTimeout(foo, 10);
  setTimeout(bar, 10);
  setTimeout(baz, 10)
  ```

- 이 코드를 실행하면

  - 아무런 지연 없이 `setTimeout` 함수가 세번 호출된 이후에 실행을 마치고 호출 스택이 비워짐
  - 10ms가 지나는 순간 `foo`, `bar`, `baz` 함수가 순차적으로 태스크 큐에 추가
  - 이벤트 루프는 `foo` 함수가 태스크 큐에 들어오자마자 호출 스택이 비어있으므로 `foo`를 실행해 호출 스택에 추가
  - `foo` 함수의 실행이 끝나고 호출 스택이 비워짐
  - 이벤트 루프가 다시 큐에서 다음 콜백인 `bar`를 가져와 실행
  - `bar`의 실행이 끝나면 마찬가지로 큐에 남아있는 `baz`를 큐에서 가져와 실행
  - `baz`까지 실행이 모두 완료되면 현재 진행중인 태스크도 없고 태스크 큐도 비어있기 때문에 이벤트 루프는 새로운 태스크가 태스크 큐에 추가될때 까지 대기

<br>

<br>

### setTimeout(fn, 0)

```
setTimeout(function() {
    console.log('A');
}, 0);
console.log('B');

// 'B' -> 'A'
```

- `setTimeout` 함수는 콜백 함수를 바로 실행하지 않고 호출 스택이 아닌 태스크 큐에 추가한다. 따라서 콘솔에 'B' -> 'A' 순서로 출력된다.
- 실행이 너무 오래걸리는 코드를 `setTimeout`을 사용해 적절하게 다른 태스크로 나누어주면 프로그램이 멈추거나 스크립트가 느려지는 상황을 막을 수 있다.
- `setTimeout` 함수의 '0'은 0ms, 즉시를 의미하는것은 아니다. 브라우저마다 내부적으로 타이머의 최소 단위(tick)가 있기때문에 그 최소 단위를 지난 후에 태스크 큐에 추가되게 된다.



<br>

<br>

<br>


------

**Reference**

- [JavaScript Event Loop Explained](https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4)
- [자바스크립트와 이벤트 루프](https://meetup.toast.com/posts/89)
- [자바스크립트 엔진, Event Loop, Web API, Task Queue](https://velog.io/@ksh4820/Event-Loop)



