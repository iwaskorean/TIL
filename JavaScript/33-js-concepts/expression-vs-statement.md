# Expression vs Statement

<br>

### Expression

> Expressions would always return a single value.

- 어떤 값을 반환하는 식

- state를 바꾸지 않더라도 표현식을 통한 값 반환이 가능하다.

  ```
  2 + 2 * 3 / 2
  
  (Math.random() * (100 - 20)) + 20
  
  1+1, 2+2, 3+3
  // "," 쉼표를 사용해 여러 표현식을 나열 할 수 있다. 그러나 반환은 마지막 표현식의 값만 반환한다.
  ```

  ```
  // Expressions don't necessarily change a state.
  
  const assignedVariable = 2; // assignedVariable is state
  assignedVariable + 4 // expression
  assignedVariable - 10 // expression
  console.log(assignedVariable) // 2
  ```

  ```
  3 * 3 // expression
  
  function multiply(n1, n2) {
  	n1 * n2;
  }
  
  const result = multiply(3, 3); //expression, undefined
  // multiply(3,3)은 undefined를 반환하므로 이 또한 표현식이다.
  ```

<br>

### Statement

> Statements can never be used where a value is expected.

- Statement는 값이 예상되는 곳이 사용 될 수 없는 명령이나 지시를 뜻한다.

  ```
  console.log(if(true) {3+3}); // error
  ```

- 자바스크립트에서의 Statement 종류

  - `if`
  - `if-else`
  - `while`
  - `do-while`
  - `for`
  - `switch`
  - `for-in`
  - with(deprecated)
  - debugger
  - variable declaration

<br>

<br>

### Function Declaration

- `function` 키워드로 선언된 함수

- 선언된 함수는 나중 사용을 위해(saved for later use) 저장 되었다가 호출될때 실행

- Function declaration은 코드가 실행되기 전 load (= hositing)

  ```
  alert(foo()); // Alerts 5. Declarations are loaded before any code can run.
  function foo() { return 5; }
  ```

<br>

### Function Expression

- 표현식을 이용한 함수 정의

- 함수 표현식은 변수에 저장될 수 있다, 저장 된 후에 변수는 함수를 사용 하는 방식대로 사용될 수 있다.

- Function declaration과 달리 표현식으로 정의된 함수는 호이스팅이 되지 않고 인터프리터가 해당 코드의 라인에 왔을때 로드된다.

  ```
  alert(foo()); // ERROR! foo wasn't loaded yet
  var foo = function() { return 5; }
  ```

- Function declaration보다 Function expression을 사용하는것이 더 나은 경우들

  - 클로저
  - 다른 함수의 인수로 사용할때(arguments to other function)
  - 즉시실행함수(IIFE)

<br>

<br>

<br>


------

**Reference**

- [All you need to know about Javascript's Expressions, Statements and Expression Statements](https://dev.to/promhize/javascript-in-depth-all-you-need-to-know-about-expressions-statements-and-expression-statements-5k2)
- [Function Declarations vs. Function Expressions](https://medium.com/@mandeep1012/function-declarations-vs-function-expressions-b43646042052)