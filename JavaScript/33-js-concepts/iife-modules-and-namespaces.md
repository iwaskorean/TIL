# IIFE, Modules and Namespaces

## IIFE

IIFE는 즉시 호출 함수(Immediately-Invoked Function Expression)의 약자이다. IIFE 내부에 변수를 선언함으로써 변수에 접근 하는 것을 방지 할 수 있다. 또한 IIFE는 변수에 할당한 값을 리턴 할 수 있으며 매개변수를 가질 수 있다.

<br>

```javascript
!function() {
    alert("Hello from IIFE!");
}();
// Shows the alert
```

IIFE는 `!` 연산자와 `function` 키워드를 사용해 선언할 수 있다.  `!` 키워드는 함수를 statement 대신 expression으로 처리하도록 하는 역할을 한다. `!` 대신 `+`, `-`, `~` 등의 단항연산자로 대체 가능하다.

<br>

### Classical IIFE style

```javascript
(function() {
    alert("I am not an IIFE yet!");
});
```

위의 코드는 함수 expression이 `()`로 감싸진 형태이다. 그러나 아직 함수 expression이 실행되지 않았으므로 IIFE는 아니다.

IIFE의 일반적인 스타일은 아래 코드와 같다.

```javascript
// Variation 1
(function() {
    alert("I am an IIFE!");
}());

// Variation 2
(function() {
    alert("I am an IIFE, too!");
})();
```

세부적으로는 두 형태의 작동방식이 조금 다르지만, 같은 동작을 하는 IIFE로 생각하면 된다.

<BR>

### IIFE with Private values

IIFE 내부에 변수나 함수를 선언함으로써 외부에서 접근하지 못하게 보호 할 수 있다.

```javascript
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

`IIFE_initgame()` 함수 내부에 변수 `lives`, `weapons`를 선언함으로써 외부에서 접근 할 수 없게 보호할 수 있다.  `init()` 함수는 `IIFE_initgame()` 내부에서 선언되었기 때문에 부모 범위에서 선언된 변수인 `lives`, `weapons`에 접근할 수 있다.

<br>

### IIFE s with a return value

IIFE는 변수에 할당된 값을 리턴 할 수 있다. 값을 리턴할 필요가 없다면 `!`로 선언한 IIFE의 형태를 사용하면 된다.

```javascript
var result = (function() {
    return "From IIFE";
}());

alert(result); // alerts "From IIFE"
```

<br>

### IIFE with parameter

```javascript
(function IIFE(msg, times) {
    for (var i = 1; i <= times; i++) {
        console.log(msg);
    }
}("Hello!", 5));
```

<br>

## Module

모듈이란 프로그램을 구성하는 일부이며 다양한 키워드를 통해 다른 모듈 내부의 함수나 변수를 불러와 사용할 수 있다.

모듈을 사용할 때의 이점은 다음과 같다.

- Maintainability(유지 보수) : 모듈은 독립적이므로 분리된 모듈의 업데이트를 통해 프로그램을 효과적으로 개선시킬 수 있다.
- Namespace : 모듈간의 개별적인 네임스페이스로 관련없는 코드 간의 전역 변수를 공유하는 namespace pollution을 방지 할 수 있다.
- Reusability(재사용성) : 재사용 가능한 모듈을 통해 더 쉽게 프로그램을 작성 할 수 있다.

<br>

### Moudule Pattern

모듈 패턴을 사용하면 단일 객체 내부에 public, private 메소드와 변수를 저장할 수 있다. 아래 코드들은 다양한 모듈 패턴의 실제 예시이다.

```javascript
// Example 1: Anonymous closure
// 익명 클로저 함수를 사용해 모듈을 만드는 패턴

(function () {
  // 변수들을 클로저 범위 내에서 private하게 유지
  var myGrades = [93, 95, 88, 0, 55, 91];
  
  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item}, 0);
    
      return 'Your average grade is ' + total / myGrades.length + '.';
  }

  var failing = function(){
    var failingGrades = myGrades.filter(function(item) {
      return item < 70;});
      
    return 'You failed ' + failingGrades.length + ' times.';
  }

  console.log(failing());

}());

// ‘You failed 2 times.’
```

```javascript
// Example 2: Object interface
// 객체 인터페이스를 통해 모듈을 만드는 패턴

var myGradesCalculate = (function () {
    
  // 변수들을 클로저 범위 내에서 private하게 유지
  var myGrades = [93, 95, 88, 0, 55, 91];

  return {
    average: function() {
      var total = myGrades.reduce(function(accumulator, item) {
        return accumulator + item;
        }, 0);
        
      return'Your average grade is ' + total / myGrades.length + '.';
    },

    failing: function() {
      var failingGrades = myGrades.filter(function(item) {
          return item < 70;
        });

      return 'You failed ' + failingGrades.length + ' times.';
    }
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

```javascript
// Example 3: Revealing module pattern
// 명시적으로 노출하려는 부분만 정해서 노출하는 노출식 모듈 패턴

var myGradesCalculate = (function () {
    
  // 변수들을 클로저 범위 내에서 private하게 유지
  var myGrades = [93, 95, 88, 0, 55, 91];
  
  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item;
      }, 0);
      
    return'Your average grade is ' + total / myGrades.length + '.';
  };

  var failing = function() {
    var failingGrades = myGrades.filter(function(item) {
        return item < 70;
      });

    return 'You failed ' + failingGrades.length + ' times.';
  };

  return {
    average: average,
    failing: failing
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

<br>

### CommonJS

commonjs 모듈의 특징은 특정한 객체를 내보내는 재사용 가능한 자바스크립트 일부분으로 다른 모듈에서 `require`로  불러 올 수 있으며  `module.export`로 내보낼 수 있다.

```javascript
function myModule() {
  this.sayHi = function() {
    return 'Hi!';
  }
}

module.exports = myModule;
```

```javascript
var myModule = require('myModule');

var myModuleInstance = new myModule
myModuleInstance.sayHi(); // 'Hi!'
```

<br>

### AMD

AMD Asynchronous Module Definition의 약자이며 비동기적 모듈 정의라는 뜻이다. 모듈의 의존성을 첫 번째 인자로 가지며 의존성이 로드된 후 콜백 함수를 호출한다.

```javascript
define(['module1', 'module2'], function(module1, module2) {
  console.log(module1.hello());
});
```

<br>

### UMD

CommonJS와 AMD의 기능을 모두 제공해야하는 경우 사용하는 모듈이다.

<br>

### Native JS(ES6)

Native js 모듈은 `export`, `import` 키워드를 이용해 불러오고 내보낼 수 있다. `as`, `*`와 같은 키워드 사용 별칭으로 사용하거나 모두 가져오는 등의 옵션을 지정할 수 있다.

CommonJS와 AMD에서 지원하지 않는 `export`에 대한 실시간 읽기 전용(read-only) 뷰를 제공한다.

```javascript
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

CommonJS일 경우에 코드는 아래와 같다.

```javascript
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


------

**Reference**

- [Essential JavaScript: Mastering Immediately-invoked Function Expressions](https://vvkchandra.medium.com/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6)
- [JavaScript Modules: A Beginner’s Guide](https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/)