#  Javascript Engines

<br>

### Javscript Engine

- 자바스크립트 엔진은 개발자가 작성한 코드를 브라우저가 해석할 수 있도록 그리고 애플리케이션 내부에 내장 될 수 있도록 빠르고 최적화된 코드로 변환한다.

- 자바스크립트 엔진의 종류
  - Spidermonkey(Firefox)
  - Chakra(Edge)
  - Javascriptcore(Safari)
  - V8(Chrome, Nodejs, Electron)

- 최근 대부분 엔진들은 인터프리터와 컴파일러의 이점인 빠른 애플리케이션의 시작과 빠른 코드 실행을 결합한 형태이다.

- ~~각 브라우저 엔진에 대한 특징과 작동방식을 파악해 특정 스크립트가 브라우저에 따라 어떤 동작 차이를 가지게 되는지 이해하는 것이 중요하다.~~ 

<br>

<br>

### Pipeline

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcV4v1j%2FbtqwLUgUBTt%2FDzVvemRa48SrvZBv5lPrtK%2Fimg.png)

- 자바스크립트 엔진은 자바스크립트 소스를 파싱해 추상 구문 트리(AST)로 만든다.

- 추상 구문 트리를 바탕으로 인터프리터는 최적화 되지 않은 바이트코드를 생성한다.

  - 위 두 과정이 실제 엔진이 자바스크립트로 작성된 코드를 실행하는 부분이다.

    <br>

- #### 인터프리터와 컴파일러 파이프라인

  - 인터프리터 : 최적화 되지 않은 바이트코드를 빠르게 생성한다.

  - 최적화 컴파일러 : 최적화된 기계어 코드를 생성한다.

<br>

<br>

### The Chrome V8 Engine

- C++로 작성되었으며 Chrome과 Node.js, Electron에서 사용된다.

- ECMA-262에 정의된대로 ECMA Script를 구현

  > - ECAM-262 : 정보통신 표준을 제정하는 비영리 표준화기구 ecma가 제정한 기술규격으로 범용 목적의 스크립트 언어에 대한 명세
  >
  > - ECMA Script : ECMA-262에서 정의된 하나의 사양 즉 스크립트 언어가 지켜야할 규칙이나 세부사항등을 정의

- 독립적으로 실행될 수 있으며 동시에 C++ 자체로 함수 구현을 통해 자바스크립트에 새로운 기능을 추가 할 수 있다.

  ![V8](https://cdn-media-1.freecodecamp.org/images/1*PA3QZ_7EWgoDGNyJID_7MA.png)
  
- 생성하는 Object를 메모리에 할당하며 더이상 사용되지 않는 Object의 메모리를 가비지 컬렉션을 이용해 해제한다.

  <br>

- #### V8에서의 실행 단계

  ![img](https://www.freecodecamp.org/news/content/images/2020/08/v8-overview-2.png)

  1. 파서가 자바스크립트 소스코드에서 추상 구문 트리를 생성

  2. V8의 인터프리터 Iginiton은 구문 트리에서 바이트 코드를 생성

  3. 바이트 코드와 피드백 데이터와 함께 컴파일러로 보냄

  4. V8의 컴파일러 TurboFan은 바이트 코드에서 그래프를 생성해 섹션을 고도로 최적화된(highly-optimized) 기계어로 대체

  5. 위 과정 중 한 부분에서 잘못됐다고 판단되면 최적화 컴파일러는 최적화를 중지(de-optimizes)하고 인터프리터로 돌아감

<br>

<br>

<br>

- ###### Note 

  - ###### The Just-in-Time (JIT) paradigm(Interpreter, Compiler), Hidden Class, Inline Cache, The Diffrence with Engines


------

**Reference**

- [JavaScript Engines](http://www.softwaremag.com/javascript-engines/)
- [Understanding How the Chrome V8 Engine Translates JavaScript into Machine Code](https://www.freecodecamp.org/news/understanding-the-core-of-nodejs-the-powerful-chrome-v8-engine-79e7eb8af964/)
- [JavaScript Engine Pipeline](https://kyoun.tistory.com/m/197)
- [How JavaScript Works: Under the Hood of the V8 Engine](https://www.freecodecamp.org/news/javascript-under-the-hood-v8/)



