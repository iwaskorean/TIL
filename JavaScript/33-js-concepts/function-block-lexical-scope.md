# Function Scope, Block Scope and Lexical Scope

자바스크립트에서 scope의 정의는 컴파일러가 변수나 함수가 필요할때 찾는 공간을 의미한다. scope는 global scope와 local scope로 나눌 수 있다.

global scope란 함수 또는 중괄호(`{}`) 외부 전역 범위이며 모든 위치에서 사용이 가능하다. local scope란 지역적인 범위의 특정 부분에서만 사용되며 function scope와 block scope로 구분된다.

<br>

### Function Scope

![img](https://i0.wp.com/css-tricks.com/wp-content/uploads/2017/08/one-way-glass.png?ssl=1)

function scope는 함수 내부 스코프를 의미하며 함수 내부에서 선언된 변수는 함수 내부에서만 접근이 가능하다.

```javascript
function sayHi () {
  const hi = 'Hi there!'
  console.log(hi)
}

sayHi() // 'Hi there!'
console.log(hi) // Error, hi is not defined
```

function scope에서 다른 함수를 호출할 수 있지만, 다른 함수 내부에서 선언된 내부 변수에는 접근이 불가하다.

```javascript
function first () {
  const firstFunctionVariable = `I'm part of first`
}

function second () {
  first() // It works
  console.log(firstFunctionVariable) // Error, firstFunctionVariable is not defined
}
```
<br>

### Function Hoisting

`function` 키워드로 선언된 함수는 항상 현재 범위의 제일 위로 호이스팅된다. 따라서 아래 두 코드의 결과는 동일하다.	

```javascript
sayHi()
function sayHi () {
  console.log('Hi there!')
}
```

```javascript
function sayHi () {
  console.log('Hi there!')
}
sayHi()
```

<br>

### Lexical Scope

lexical scope란 함수를 어디에서 선언했는지에 따라 상위 스코프가 결정된다.

> dynamic scope : Lexical Scope와 반대되는 개념으로 어디서 호출되었는지가 스코프를 결정
```javascript
var num = 1;

function foo() {
  var num = 100;
  bar();
}

function bar() {
  console.log(num);
}

foo(); // 1
bar(); // 1
```

`bar()` 함수의 상위 스코프는 lexical scope에 따라 `bar()` 함수를 선언한 위치 즉, global scope로 결정된다. 따라서 global scope의 `num` 변수의 값인 1이 로그 출력된다.

만약 lexical scope가 아닌 dynamic scope에 따라 스코프가 결정되었다면 `bar()`의 호출 위치 즉, `foo()` 함수 내부가 상위 스코프가 되어 100이 로그 출력되었을것이다.

<br>

### Closer

클로저는 함수와 환경(environment)의 조합이다. 여기서 환경이란 접근할 수 있는 모든 스코프를 뜻한다. 함수 내부에서 다른 함수를 선언할때마다 클로저가 생성되며 부모 함수의 실행이 끝났더라도 부모 함수 범위에 접근할 수 있다.

클로저는 예상하지 못한 결과를 제어하거나 private한 변수와 메소드를 만들기 위한 용도로 사용된다.

```javascript
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
// myFunc변수에 displayName을 리턴함
// 유효범위의 렉시컬한 환경을 유지

myFunc(); // alert: Mozilla
```

`makeFunc()`의 실행이 끝난 후 `myFunc()`를 호출했으므로 `name`에 접근하지 못해 위 코드가 정상적으로 작동하지 않을거라 예상하는것이 일반적일 수 있지만, 위 코드는 클로저에 의해 정상적으로 동작한다.

자바스크립트는 함수를 리턴할 때 클로저를 생성하며 클로저가 생성된 시점의 함수와 환경에 대한 참조를 유지한다. 따라서 `myFunc`가 호출될 때 클로저로 인해 `name`을 사용할 수 있는 상태가 되고 "Mozilla"가 alert에 전달된다.

<br>

```javascript
function makeAdder(x) {
  var y = 1;
  return function(z) {
    y = 100;
    return x + y + z;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)

//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

`add5`와 `add10`은 `makeAdder()`의 스코프를 공유하지만 클로저에 의해 서로 다른 환경에 대한 참조를 유지하고 있다. 클로저가 생성된 시점을 기준으로 `add5`의 환경에서 `x`는 5이고, `add10`의 환경에서는 `x`는 10이므로 위와 같은 결과가 반환된다.

<br>

```javascript
// 클로저를 이용한 private한 변수 생성

function secret (secretCode) {
  return {
    saySecretCode () {
      console.log(secretCode)
    }
  }
}

const theSecret = secret('This is amazing')
theSecret.saySecretCode()
```

<br>

### Block Scope

중괄호(`{}`) 내부에서 `let`, `const` 변수를 선언하면 그 변수들은 중괄호 내부에서만 접근이 가능하다. 함수도 중괄호로 선언되므로 block scope도 function scope의 부분집합이라고 할 수 있다.

```javascript
{
  const hi = 'Hi there!'
  console.log(hi) // 'Hi there!'
}

console.log(hi) // Error, hi is not defined
```

<br>

<br>


------

**Reference**

- [JavaScript Scope and Closures](https://css-tricks.com/javascript-scope-closures/)
- [Understanding Scope in JavaScript](https://www.telerik.com/blogs/understanding-scope-in-javascript)
- [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)