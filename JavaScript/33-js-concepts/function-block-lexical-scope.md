# Function Scope, Block Scope and Lexical Scope

<br>

### Scope의 정의

- 컴파일러가 변수나 함수가 필요할때 찾는 공간
- Global scope, Local scope 두 종류로 나눌수 있다.
  - Global scope 
    - 함수 또는 중괄호 `{}` 외부에서 선언, 모든 위치에서 사용 가능
  - Local scope 
    - 지역적인 범위의 특정 부분에서만 사용
    - Function Scope와 Block scope로 구분된다.

<br>

### Function Scope

- 함수내에서 변수를 선언할때, 함수 내부에서만 변수 접근이 가능하다.

  ```
  function sayHi () {
    const hi = 'Hi there!'
    console.log(hi)
  }
  
  sayHi() // 'Hi there!'
  console.log(hi) // Error, hi is not defined
  ```

- 함수 범위에서 다른 함수를 호출 할 수 있지만 다른 함수 내부 변수에 접근은 불가

  ```
  function first () {
    const firstFunctionVariable = `I'm part of first`
  }
  
  function second () {
    first() // It works
    console.log(firstFunctionVariable) // Error, firstFunctionVariable is not defined
  }
  ```
  ![img](https://i0.wp.com/css-tricks.com/wp-content/uploads/2017/08/one-way-glass.png?ssl=1)

<br>

### Function Hoisting

- `function` 키워드로 선언된 함수는 항상 현재 범위의 제일 위로 hoist, 따라서 아래 두 코드의 결과는 동일하다.	

  ```
  sayHi()
  function sayHi () {
    console.log('Hi there!')
  }
  ```

  ```
  function sayHi () {
    console.log('Hi there!')
  }
  sayHi()
  ```

<br>

### Lexical Scope and Closer

- #### Lexical Scope

  - 렉시컬 범위  : 함수를 어디에서 호출했는지에 따라 범위가 결정되는 것이 아닌 함수를 어디에 선언했는지에 따라서 범위가 결정된다.

  > *cf )* 
  >
  > Dynamic Scope : Lexical Scope와 반대되는 개념으로 어디서 호출되었는지가 범위를 결정
  ```
  function init() {
    var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.
    function displayName() { // displayName() 은 내부 함수이며, 클로저다.
      alert(name); // 부모 함수에서 선언된 변수를 사용한다.
    }
    displayName();
  }
  init();
  ```

  - `displayName()` 내부엔 자신만의 지역 변수가 없지만 함수 범위 내부에서 선언된 또 다른 함수에서 부모 함수 범위의 변수에 접근할 수 있으므로 `displayName()` 역시 부모 함수 `init()`에서 선언된 변수 `name`에 접근할 수 있다. 

    <br>

- #### Closer

  - 함수 내부에서 다른 함수를 선언할때마다 클로저가 생성

  - 클로저는 부모 함수의 실행이 끝났더라도 부모 함수 범위에 접근할 수 있다.

  - 클로저는 예상하지 못한 결과를 제어하거나 private한 변수와 메소드를 만들기 위한 용도로 사용된다.

    ```
    function makeFunc() {
      var name = "Mozilla";
      function displayName() {
        alert(name);
      }
      return displayName;
    }
    
    var myFunc = makeFunc();
    //myFunc변수에 displayName을 리턴함
    //유효범위의 렉시컬한 환경을 유지
    myFunc();
    //리턴된 displayName 함수를 실행(name 변수에 접근)
    ```

    - 자바스크립트는 함수를 반환하고 반환하는 함수가 클로저를 형성하기 때문에 `displayName()`함수가 실행되기 전에 외부함수인 `makeFunc()`로부터 리턴되어 `myFunc` 변수에 저장

    ```
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

    - `add5`와 `add10`은 모두 클로저, `makeAdder()`의 범위를 공유하지만 서로 다른 렉시컬한 환경을 저장한다.
    - `add5`의 렉시컬한 환경에서 x는 5이지만, `add10`의 렉시컬한 환경에서의 x는 10

    ```
    // 클로저를 이용한 private한 변수 활용
    
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

  - 중괄호 `{}` 내부에서 `let`, `const` 변수를 선언할때 그 변수들은 `{}` 내부에서만 접근 가능

  - 함수도 `{}`로 선언됨으로 블록 범위도 함수 범위의 부분 집합(subset)

    ```
    {
      const hi = 'Hi there!'
      console.log(hi) // 'Hi there!'
    }
    
    console.log(hi) // Error, hi is not defined
    ```

  <br>

  <br>
  
  <br>


------

**Reference**

- [JavaScript Scope and Closures](https://css-tricks.com/javascript-scope-closures/)
- [Understanding Scope in JavaScript](https://www.telerik.com/blogs/understanding-scope-in-javascript)
- [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)