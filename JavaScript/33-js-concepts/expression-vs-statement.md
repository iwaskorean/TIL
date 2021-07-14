# Expression vs Statement

자바스크립트에는 expression과 statement라는 주요한 두 가지 구문 카테고리(syntactic categories)가 있다. 

expression은 statement처럼 동작할 수 있지만 statement는 expression처럼 동작할 수 없다.

<br>

### Expression

expression은 값을 반환하는 구문이다. 

```javascript
2 + 2 * 3 / 2

(Math.random() * (100 - 20)) + 20

1+1, 2+2, 3+3
// "," 쉼표를 사용해 여러 표현식을 나열 할 수 있다. 그러나 마지막 표현식의 값만 반환한다.
```

<br>

expression은 state를 변경하지 않는다. 

아래 코드 예제의 expression에서 `assignedVariable`이라는 state에 `+`, `-` 연산을 했지만 해당 연산은 반환 값에만 영향을 줄 뿐 실제 `assignedVariable` 값에는 영향을 주지 않는다.

```javascript
const assignedVariable = 2; // statement, assignedVariable은 state
assignedVariable + 4 // expression, 
assignedVariable - 10 // expression

console.log(assignedVariable) // 2
```

```javascript
3 * 3 // expression

function multiply(n1, n2) {
	n1 * n2;
}

const result = multiply(3, 3); //expression, undefined

// multiply(3,3)은 undefined를 반환하므로 이 또한 표현식이다.
```

<br>

<br>

### Statement

statement는 값이 필요한 곳에 사용될 수 없는 명령이나 지시 구문을 의미한다. 

즉, statement는 함수의 인자,  할당 시 오른쪽 부분, 피연산자, 반환 값 등으로 사용될 수 없다. 

```javascript
console.log(if(true) {3+3}); 
// Uncaught SyntaxError: Unexpected token 'if'
```

예를 들어 콘솔에 위 코드를 입력 후 실행할 경우 콘솔에 에러가 출력 될 것이다. 

<br>

자바스크립트에서의 statement의 종류 아래와 같다.

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

function declaration이란 `function` 키워드를 사용한 함수 선언이다.

function declaration으로 선언된 함수는 향후 사용을 위해(saved for later use) 저장 되었다가, 호출 될 때 실행된다. 또한 function declaration으로 선언된 함수는 코드가 실행 되기 전 로드(호이스팅)된다.

```javascript
alert(foo()); // Alerts 5

function foo() { return 5; }

// function foo()는 function declaration이므로 호이스팅되어 함수 선언이 호출보다 뒷 라인에 위치하더라도 정상적으로 실행된다.
```

<br>

### Function Expression

function expression은 표현식을 사용한 함수 선언이다.

function exrpression은 변수에 저장될 수 있으며 저장된 후 함수를 사용하는 방식대로 사용될 수 있다.

Function declaration과 달리 function expression으로 정의된 함수는 호이스팅이 되지 않고 인터프리터가 해당 코드의 라인에 왔을 때 로드된다.

```javascript
alert(foo()); // foo 함수는 아직 로드되지 않았으므로 에러가 출력된다.

var foo = function() { return 5; }
```

다른 함수의 인자로 사용, 클로저, 즉시실행함수(IIFE) 등의 목적으로 함수를 선언할 때에는 function declaration보다 function expression으로 함수를 선언하는 것이 더 유용하다.

<br>

<br>


------

**Reference**

- [All you need to know about Javascript's Expressions, Statements and Expression Statements](https://dev.to/promhize/javascript-in-depth-all-you-need-to-know-about-expressions-statements-and-expression-statements-5k2)
- [Function Declarations vs. Function Expressions](https://medium.com/@mandeep1012/function-declarations-vs-function-expressions-b43646042052)