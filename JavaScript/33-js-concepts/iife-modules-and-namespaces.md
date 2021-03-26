# IIFE, Modules and Namespaces

<br>

### IIFE

- 즉시 호출 함수(Immediately-Invoked Function Expression)

- IIFE 내부에 변수를 선언함으로써 변수에 접근 하는 것을 방지 할 수 있다.

- IIFE는 변수에 할당한 값을 리턴 할 수 있으며 매개변수를 가질 수 있다.

  <br>

- ##### `!`를 이용한 IIFE의 형태

  ```
  !function() {
      alert("Hello from IIFE!");
  }();
  // Shows the alert
  ```

  - `!` 대신 `+`, `-`, `~` 등의 단항연산자로 대체 가능하다.

  - `!` 키워드는  자바스크립트가 `!` 다음에 어떤것이 오든 표현식으로 treat 하게 강제한다.

    <br>

- ##### 일반적인 IIFE의 형태

  ```
  (function() {
      alert("I am not an IIFE yet!");
  });
  ```

  - 위의 코드는 함수 표현식이 `()`로 감싸진 형태이다. 그러나 아직 함수 표현식이 실행되지 않았으므로 IIFE는 아니다.
  - IIFE의 일반적인 두 형태는 아래 코드와 같다.

  ```
  // Variation 1
  (function() {
      alert("I am an IIFE!");
  }());
  
  // Variation 2
  (function() {
      alert("I am an IIFE, too!");
  })();
  ```

  - 세부적으로는 두 형태의 작동방식이 조금 다르지만, 다른 형태를 가진 같은 동작을 하는 IIFE로 생각

    <BR>

- ##### IIFE with Private values

  - IIFE 내부에 변수나 함수를 선언함으로써 외부에서 접근하지 못하게 보호 할 수 있다.

  ```
  (function IIFE_initGame() {
      // 외부에서 접근 할 수 없는 private values
      var lives;
      var weapons;
      
      init();
  
      // 외부에서 접근 할 수 없는 private function
      function init() {
          lives = 5;
          weapons = 10;
      }
  }());
  ```

  - `IIFE_initgame()` 내부에 `lives`, `weapons`를 선언 함으로써 외부에서 접근 할 수 없게 보호 할 수 있다.

  - `IIFE_initgame()` 내부에 `init()` 함수를 선언했기때문에 부모 범위인 `IIFE_initgame()` 내부의 변수  `lives`, `weapons`에 접근 할 수 있다.

    <br>

- ##### IIFE s with a return value

  - IIFE는 변수에 할당된 값을 리턴 할 수 있다.

  - 값을 리턴할 필요가 없다면 `!`을 이용한 IIFE의 형태를 사용

    ```
    var result = (function() {
        return "From IIFE";
    }());
    
    alert(result); // alerts "From IIFE"
    ```

    <br>

- ##### IIFE with parameter

  ```
  (function IIFE(msg, times) {
      for (var i = 1; i <= times; i++) {
          console.log(msg);
      }
  }("Hello!", 5));
  ```

<br>

<br>

### Module

- 프로그램을 구성 하는 일부

- `import`나 `export` 지시자를 통해 다른 모듈 내부의 함수나 변수를 불러와 사용 할 수 있음

  <br>

- ##### 모듈을 사용하는 이유

  - Maintainability(유지 보수)
    - 모듈은 독립적이므로 분리된 모듈의 업데이트를 통해 프로그램을 효과적으로 개선시킬 수 있다.
  - Namespace
    - 모듈간의 개별적인 네임스페이스로 관련없는 코드 간의 전역 변수를 공유하는 namespace pollution을 방지 할 수 있다.
  - Reusability(재사용성)
    - 재사용 가능한 모듈을 통해 더 쉽게 프로그램을 작성 할 수 있다.

  <br>

### Moudule Pattern

- 일부 범위를 부모(전역) 범위로 부터 보호 할 수 있다.

- 자체적인 네임스페이스를 만듦으로써 변수나 함수의 이름 충돌을 방지 할 수 있다.

  <br>

- #### CommonJS

  - 특정한 객체를 내보내는 재사용가능한 자바스크립트 일부분으로 다른 모듈에서 `require`로  불러올수 있고  `module.export`로 내보낼 수 있다.

    ```
    function myModule() {
      this.sayHi = function() {
        return 'Hi!';
      }
    }
    
    module.exports = myModule;
    ```

    ```
    var myModule = require('myModule');
    
    var myModuleInstance = new myModule();
    myModuleInstance.sayHi(); // 'Hi!'
    ```

    <br>

- #### AMD

  - 비동기적 모듈 정의(Asynchronous Module Definition)

    ```
    define(['module1', 'module2'], function(module1, module2) {
      console.log(module1.hello());
    });
    ```

    - 첫번째 인자로 모듈의 dependencies를 갖고 dependencies가 로드 된후 콜백 함수 호출

    <br>

- #### UMD

  - CommonJS와 AMD의 기능을 모두 제공해야하는 경우 사용

    <br>

- #### Native JS(ES6)

  - `export`와 `import` 키워드를 이용해 불러오고 내보낼 수 있다.

  - `as`, `*`와 같은 키워드 사용 별칭으로 사용하거나 모두 가져오는 등의 옵션을 지정할 수 있다.

  - CommonJS와 AMD에서 지원하지 않는 `export`에 대한 실시간 읽기 전용 뷰 제공(live read-only view)

    ```
    // lib/counter.js
    export let counter = 1;
    
    export function increment() {
      counter++;
    }
    }
    
    // src/main.js
    import * as counter from '../../counter';
    
    counter.increment();
    console.log(counter.counter); // 2
    ```

    - CommonJS일 경우에는

      ```
      var counter = 1;
      
      function increment() {
        counter++;
      }
      
      module.exports = {
        counter: counter,
        increment: increment,
      };
      
      // src/main.js
      var counter = require('../../lib/counter');
      
      counter.increment();
      console.log(counter.counter); // 1
      ```

<br>

<br>

<br>


------

**Reference**

- [Essential JavaScript: Mastering Immediately-invoked Function Expressions](https://vvkchandra.medium.com/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6)
- [JavaScript Modules: A Beginner’s Guide](https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/)