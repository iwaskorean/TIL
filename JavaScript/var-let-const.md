# var, let and const

ES6(ES2015) 이전에는 var 키워드로만 변수 선언이 가능했다. 그러나 ES6에서 변수 선언을 위한 키워드인 let과 const가 추가되었다.

<br>

## var

ES6 이전에는 변수 선언시 var 키워드를 사용할 수 밖에 없었다. 그러나 var 변수와 관련한 많은 이슈가 있었기 때문에 새로운 변수 선언 방법에 대한 필요성이 대두되었다.

<br>

### Scope of var

var 변수가 함수 외부에서 선언될 경우 전역 스코프를 가지며 함수 내부에서 선언될 경우 함수/지역 스코프를 가진다.

```javascript
var hi = 'hey hi';

function newFunction() {
  var hello = 'hello';
  console.log(hi);
}

newFunction(); // hey hi
console.log(hello); // Uncaught ReferenceError: hello is not defined
```

함수 외부에서 선언된 `hi`는 전역 범위를 가지므로 `newFunction()` 내부에서 사용이 가능하지만, `hello`는 함수 내부에서 선언되었으므로 전역 범위에서 사용할 수 없다.

<br>

### var variables can be re-declared and updated

var로 선언된 변수는 재선언 및 재할당이 가능하다.

```javascript
var city = 'seoul';
var city = 'busan';
city = 'donghae';
```

<br>

### Hoisting of var

var 변수는 코드 실행 전 변수, 함수 선언을 해당 스코프 맨 위로 이동 시키는 호이스팅이 발생한다.

```javascript
console.log(city);
var city = 'busan';

// undefined
```

위 코드는 아래와 같이 해석된다.

```javascript
var city;
console.log(city);
city = 'busan';
```

var 변수는 해당 스코프 맨 위로 호이스트되고 `undefined` 값으로 초기화된다.

<br>

### Problem with var

```javascript
var city = 'seoul';
var times = 4;

if (times > 3) {
  var city = 'busan';
}

console.log(city); // "busan"
```

`times > 3`은 `true`를 반환하므로 `city`의 값이 `busan`으로 변경되었다. `city` 변수를 어느 위치에서 사용하느냐에 따라 값이 변할 수 있다.

var 변수는 재선언 및 할당이 가능하므로 코드 실행 순서 및 스코프에 따라 변수의 값이 예상치 못하게 변할 수 있으며 이는 버그를 발생시킬 수 있다.

이것이 `let`과 `const`를 통한 변수 선언이 필요한 이유이다.

<br>

## let

let은 ES6 이후 가장 선호되는 변수 선언 키워드이다. let 키워드를 사용한 변수 선언을 통해 var 변수의 문제점을 해결할 수 있다.

<br>

### let is block scoped

let은 블록 스코프를 따른다. 블록 스코프란 `{}` 내부의 범위를 의미한다. 따라서 let으로 선언된 변수는 해당 블록 내에서만 사용할 수 있다.

```javascript
let city = 'seoul';
let times = 4;

if (times > 3) {
  let city = 'busan';
}

console.log(city); // "seoul"
```

let으로 선언된 변수는 블록 스코프이기 때문에 `if`문 내부의 `city` 변수는 `if`문 내부에서만 동작한다.

<br>

### let can be updated but not re-declared

var 변수와 달리 let 변수는 재할당이 가능하지만, 같은 스코프에서 재선언이 불가하다.

```javascript
let city = 'seoul';
 city = 'busan';

console.log(city) // 'busan'
```

```javascript
let city = 'seoul';
let city = 'busan';

// Uncaught SyntaxError: Identifier 'city' has already been declared
```

<br>

### Hoisting of let

var와 마찬가지로 let 변수 또한 호이스팅이 발생한다. 그러나 var 변수는 호이스팅과 동시에 `undefined`로 초기화되는 반면, let 변수는 초기화되지 않는다. 따라서 let 변수를 선언 이전에 사용하면  `ReferenceError`가 발생한다.

```javascript
console.log(city);
let city = 'seoul';

// Uncaught ReferenceError: Cannot access 'city' before initialization
```

<br>

## const

const 변수는 상수 값을 유지하는 변수이다. let 변수와 유사한 점이 많다.

<br>

### const declarations are block scoped

let 변수와 마찬가지로, const 변수 또한 블록 스코프 범위를 가진다.

<br>

### const cannot be updated or re-declared

const 변수는 var, let 변수와 다르게 값을 재할당할 수 없다. 같은 스코프에서 재선언 또한 불가하다.

```javascript
const city = 'busan';
city = 'seoul';

// Uncaught TypeError: Assignment to constant variable.
```

```javascript
const city = 'busan';

if (true) {
  const city = 'seoul';
}

console.log(city); // 'busan'
```

```javascript
const city;
city = 'seoul';

// Uncaught SyntaxError: Missing initializer in const declaration
```

const 변수는 재할당이 불가하므로 선언시 초기화되어야한다.

const 변수가 단일 값이 아닌 객체일 경우 객체 내부의 프로퍼티는 변경이 가능하다.

```javascript
const korea = {
  city: 'seoul',
};

korea.city = 'busan';

console.log(korea.city); // 'busan'
```

<br>

### Hoisting of const

const 변수는 let 변수와 마찬가지로 최상단으로 호이스트 되지만 초기화 되지않는다.

<br>

<br>

------

**Reference**

- [Var, Let, and Const – What's the Difference?](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)