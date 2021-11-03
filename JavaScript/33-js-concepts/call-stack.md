# Call Stack

자바스크립트는 호출 스택이 하나인 싱글 스레드 언어이며 한번에 1개의 작업만을 다룰 수 있다.

<br>

### JavaScript Engine

![heap](https://t1.daumcdn.net/cfile/tistory/99747F465C3214AF30)

자바스크립트엔진은 하나의 호출 스택(call stack)과 힙(heap)으로 구성된 싱글 스레드이다.

<br>

### Call Stack

호출 스택은 함수의 호출을 기록한 자료구조이다. 코드가 실행될 때 호출 스택이 쌓이게 된다.

함수가 실행되면 스택에 push, 반환될 때 스택의 맨위를 pop 하는 LIFO(Last in Frist Out)로 처리된다.

```javascript
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```

![img](https://t1.daumcdn.net/cfile/tistory/9995544C5C32151627)

명령이 순서대로 스택에 쌓인 뒤에 후입선출 구조에 따라 명령이 실행된다.

<br>

### Memory Heap

힙은 비구조화(unstructure) 메모리이며 힙 내부에서 변수와 객체의 할당이 일어난다. 

<br>

### Queue

자바스크립트 런타임은 메시지 큐를 포함한다

- 런타임(runtime) : 프로그래밍 언어가 구동되는 환경
- 메시지 큐(message queue) : 실행될 콜백함수나 메시지들에 대한 리스트

스택이 충분한 capacity를 가지고 있을때 메시지는 큐 밖으로 나오고, 메시지가 구성하고 있던 함수들이 실행(이 과정에서 초기 스택프레임이 만들어짐)

<br>

### Event Loop

![el](https://miro.medium.com/max/1050/1*-MMBHKy_ZxCrouecRqvsBg.png)

이벤트 루프는 비동기 처리를 하도록 하는 중요한 요소이며 호출 스택과 콜백 큐(callback queue)의 상태를 확인해 호출 스택이 빈 상태가 되면 콜백 큐의 첫번째 콜백을 호출 스택으로 밀어넣게 된다.

> 콜백 큐(Callback Queue) : 비동기적으로 실행된 콜백함수 보관

자바스크립트는 싱글 스레드 언어이기 때문에 한번에 하나의 작업만 처리할 수 있지만 이벤트 루프를 통한 비동기 처리를 통해 여러 작업을 동시에 처리할 수 있다.

이벤트 루프 포스트 : [Message Queue and Event Loop](https://github.com/SewookHan/TIL/blob/main/JavaScript/33-js-concepts/message-queue-and-event-loop.md)

<br>

######   +  실행 컨텍스트

------

**Reference**

- [Javascript executions tasks event loop call stack more part1](https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec)
- [Ultimate guide tot execution context hoising scopes and closures in js](https://ui.dev/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/)
- [Event loop](https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84)