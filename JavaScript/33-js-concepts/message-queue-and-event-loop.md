# Message Queue and Event Loop

자바스크립트는 싱글 스레드 기반의 언어이며 기본적으로 한 번에 하나의 작업만 처리할 수 있다. 비동기 요청과 동시성(concurrency)에 대한 처리는 자바스크립트 언어 자체가 아닌 자바스크립트가 구동되는 브라우저 환경에서 처리된다.

<br>

### Basic Architecture

![a](https://miro.medium.com/max/1050/1*7GXoHZiIUhlKuKGT22gHmA.png)

브라우저 환경은 자바스크립트 엔진, WebAPIs, callback queue, event loop로 구성된다.

<br>

### JS Engine

자바스크립트 엔진은 메모리 할당이 일어나는 영역인 힙과 콜 스택으로 구성되어 있다.

- 힙(heap) : 객체가 할당되는 구조화되지 않은(unstructured) 메모리 영역이다.
- 스택(stack) : 함수가 실행되는 순서를 기억하는 자료구조이다. 함수가 호출되면 인터프리터가 함수를 스택에 push하고 실행이 종료되면 스택에서 pop 되어진다. 콜 스택에서 함수를 재귀적으로 계속해서 호출할 경우 브라우저는 콜 스택 size가 exceeded 됐다는 에러와 함께 실행을 중지한다. 

<br>

### WebAPIs 

브라우저에 내장되어 있으며 DOM, ajax, HTTP 요청, setTimeout 등의 API를 제공한다.

WebAPI는 작동이 완료되면 콜백을 콜백 큐에 push한다.

<br>

### Callback queue(message queue, event queue, task queue)

콜백 큐는 스택에서 WebAPI에 의해 보내진 콜백들이 대기하는 큐이다.

<br>

### Event loop

이벤트 루프는 콜스택과 테스크 큐를 주시하며 스택이 비어있으면 큐의 첫번째 콜백을 쌓아 효과적으로 실행할 수 있게 해준다.

스택이 비어있고 콜백 큐의 콜백이 있을 경우 콜백을 스택에 push하며 스택에서 콜백이 실행된다.

<br>

### Example

```javascript
function main(){
  console.log('A');

  setTimeout(
    function exec(){ 
      console.log('B'); 
    }, 0);
  
  console.log('C');
}

main();

//	Output
//	A
//	C
//  B
```

![img](https://miro.medium.com/max/2000/1*64BQlpR00yfDKsXVv9lnIg.png)

1. `main()` 함수가 스택에 push된다. 이후 브라우저는 `main()` 함수의 첫 번째 명령문인 `console.log('A')`를 스택으로 push한 다음 실행해 `A`가 콘솔에 출력되고 `conosle.log('A')`가 스택에서 pop 된다.
2. 다음 명령문인 `setTimeout()`이 스택에 push되고 실행된다. `setTimeout()` 함수는 브라우저의 WebAPI를 사용해 콜백을 지연시키고 타이머에 대해
3. `exec()`의 콜백 타이머가 브라우저 API(WebAPI)에서 실행되는 동안 `console.log('C')`가 스택으로 push 된다. 이 예제의 경우 타이머가 0ms 이므로 브라우저가 콜백을 받는 즉시 콜백 큐에 콜백이 추가된다.
4. `main()` 함수의 마지막 명령문인 `console.log('C')`가 실행되어 콘솔에 `C`가 출력되고 `main()` 함수가 스택에서 pop 된다.
5. 현재 콜 스택이 비어있는 상태이므로 이벤트 루프에 의해 콜백 큐에 대기하고 있던 콜백 `exec()`가 스택으로 push되고 `console.log('B')` 실행되어 콘솔에 `B`가 출력된다.

<br>

<br>


------

**Reference**

- [JavaScript Event Loop Explained](https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4)

- [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

  

