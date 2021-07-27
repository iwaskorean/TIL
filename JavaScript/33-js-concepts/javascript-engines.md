#  Javascript Engines

자바스크립트 엔진은 개발자가 작성한 자바스크립트 코드를 브라우저가 해석 가능하고 어플리케이션에 내장할 수 있게 빠르고 최적화된 코드로 변환시키는 역할을 한다.

브라우저별 자바스크립트 엔진의 종류는 다음과 같다.

- Firefox : Spidermonkey
- Edge : Chakra
- Safari : JavaScriptCore
- Chrome : V8

<br>

## V8 Engine

V8은 구글에서 제공하는 오픈소스 자바스크립트 엔진이다. C++로 작성되었으며 Chrome과 Node.js, Electron에서 사용된다. ECMA-262에 정의된대로 ECMAScript를 구현한다.

> - ECAM-262 : 정보통신 표준을 제정하는 비영리 표준화기구 ecma가 제정한 기술규격으로 범용 목적의 스크립트 언어에 대한 명세이다.
>
> - ECMAScript : ECMA-262에서 정의된 하나의 사양 즉, 스크립트 언어가 지켜야할 규칙이나 세부사항 등을 정의한다.

V8은 독립적으로 실행될 수 있으며 C++의 자체적인 함수 구현을 통해 자바스크립트에 새로운 기능을 추가할 수 있다.

![V8](https://cdn-media-1.freecodecamp.org/images/1*PA3QZ_7EWgoDGNyJID_7MA.png)

예를 들어 `print('hello world')`는 Node.js에서 유효한 명령문이 아니며 이 명령문을 컴파일할 경우 오류가 발생한다. 그러나 V8 오픈소스에서 C++로 `print()` 함수를 구현해 추가하면 정상적으로 동작하도록 할 수 있다.

<br>

## How V8 engine works

### Parsing

파싱이란 소스 코드를 불러온 후 AST로 변환하는 과정이다. AST는 소스 코드를 컴퓨터가 이해하기 쉽게 구조화화된 자료 구조이다.

### Ignition 

Ignition은 자바스크립트를 바이트 코드로 변환하는 인터프리터이다.

소스 코드를 컴퓨터가 해석하기 쉬운 바이트 코드로 변환해 원본 코드를 닫시 파싱해야하는 번거로움을 줄이고 코드의 양도 줄여서 코드 실행 시 차지하는 메모리 공간을 아낄 수 있다.

### TurborFan

TurborFan은 최적화를 담당하는 컴파일러이다. 런타임을 구성하는 요소중 하나인 Profiler에게 함수나 변수의 호출 빈도와 같은 여러 데이터를 수집 받아 기준에 충족하는 코드들을 최적화한다.

<br>

### Compile Pipeline

실제 V8에서의 컴파일 과정은 아래와 같다.

![I](https://evan-moon.github.io/static/27979b3d2674a00f5b68af5f303fb27c/d9199/v8compiler-pipeline.png)

1. V8은 소스 코드를 가져와 파서로 보낸다. 
2. 파서는 소스 코드를 분석한 후 AST(추상 구문 트리)로 변환한다. 
3. AST를 Ignition로 전송한다. 
4. 바이트 코드를 실행함으로써 소스 코드가 실제로 작동하며 자주 사용되는 코드는 TurboFan으로 보내져 최적화 코드(Optimized Machine Code)로 컴파일된다.
5. 최적화 코드의 사용빈도가 적어지면 de-optimizing가 되기도 한다.

<br>

<br>


------

**Reference**

- [JavaScript Engines](http://www.softwaremag.com/javascript-engines/)
- [V8 엔진은 어떻게 내 코드를 실행하는 걸까?](https://evan-moon.github.io/2019/06/28/v8-analysis/)

