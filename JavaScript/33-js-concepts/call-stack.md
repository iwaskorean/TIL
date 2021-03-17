# Call Stack



- 자바스크립트 엔진은 Memory Heap과 Call Stack으로 구성

- 자바스크립트는 싱글스레드(하나의 Heap과 Callstack)로 1개의 동시성만을 다루는 언어

- 기본적으로 한번에 1개의 작업만 다룰수 있음(비동기 Callback 제외)

  

#### 1. Call Stack

- 함수의 호출을 기록한 자료구조
- 함수가 실행되면 stack에 push, return될때 stack의 맨위를 pop
- Last In First Out



#### 2. Heap

- 객체들을 힙 내부에 할당
- 힙은 비구조화(unstructure) 메모리
- 변수와 객체의 할당이 일어남
- 메모리 힙에 선언한 변수와 객체가 들어있다고 생각



#### 3. Queue

- 자바스크립트 런타임은 메시지 큐를 포함

  > 런타임(runtime) : 프로그래밍 언어가 구동되는 환경
  >
  > 메시지 큐(message queue) : 실행될 콜백함수나 메시지들에 대한 리스트

- 스택이 충분한 capacity를 가지고 있을때 메시지는 큐 밖으로 나오고, 메시지가 구성하고 있던 함수들이 실행(이 과정에서 초기 스택프레임이 만들어짐)



#### 4. Event Loop

- 비동기 함수를 이용?
  - CTA(Call To Action) 버튼이나 rendering이 필요한 무언가를 클릭하면 stack이 block된 상태이므로 아무 실행이 되지 않음
  - 싱글스레드인 자바스크립트에서 어떠한 값을 반환하기 전까지는 불가능함
  - 비동기 콜백을 이용한다는것은 코드의 일부분을 실행시키고 나중에 실행될 콜백함수를 스택에 push
  - 모든 비동기 콜백들은 코드에서 읽히자마자 바로 실행되지 않고 잠시후에 실행
  - 따라서 다른 동기 함수들과는 다르게 바로 스택 내부로 push 되지 않는다!

- Call Stack과 Callback Queue의 상태를 확인해 Call Stack이 빈 상태가 되면 Callback Queue의 첫번째 콜백을 Call Stack으로 push

  > 콜백 큐(Callback Queue) : 비동기적으로 실행된 콜백함수 보관



#### 5. Execution Context

- 목적 : 자바스크립트 엔진이 코드 해석 및 실행의 복잡성을 관리 할 수 있음

- 전역 실행 컨텍스트(Global Execution Context) : 자바스크립트 엔진이 실행될때 첫번째 실행 컨텍스트

  - 프로그램에 코드를 추가 전에 전역 실행 컨텍스트는 global object와 this 두가지로 구성

- Phase1 : Global Creation

  - global object 생성
  - this 생성 
  - 변수와 함수를위한 메모리 공간을 설정
  - 함수 선언을 메모리에 할당하는동안 변수에 기본값 undefined을 할당

- Phase2 : Execution

  - JavaScript 엔진이 코드를 한 줄씩 실행하기 시작하고 이미 메모리에 있는 변수에 실제 값을 할당

    

------

**Reference**

- [Javascript executions tasks event loop call stack more part1](https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec)
- [How does javascript actually work part 1](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)
- [Ultimate guide tot execution context hoising scopes and closures in js](https://ui.dev/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/)
- [Event loop](https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84)