# Primitive Types

![i](https://i0.wp.com/codezup.com/wp-content/uploads/2020/08/image-20.png?w=750&ssl=1)

자바스크립트에서 원시 타입(primitive data type)이란 객체가 아니며 메소드를 가지지 않는 자료형이다. string, number, bigint, boolean, undefined, symbol, null 총 7가지의 원시 타입이 있다.

<br>

### All primitives are immutable

원시 타입은 immutable하다. 이것은 원시 타입 값이 할당된 변수가 변경될 수 없다는 것이 아니라 원시 타입 값 자체가 객체, 배열, 함수가 변경되는 방식으로 변경될 수  없다는것을 의미한다.

```javascript
// 문자열 메소드는 문자열을 변경하지 않는다.
var bar = "baz";
console.log(bar);               // baz
bar.toUpperCase();
console.log(bar);               // baz

// 배열 메소드는 배열 값을 변경한다.
var foo = [];
console.log(foo);               // []
foo.push("plugh");
console.log(foo);               // ["plugh"]

// 할당(assignment)은 원시 값을 변형하는 것이 아닌 새로운 값을 부여한다.
bar = bar.toUpperCase();       // BAZ
```

<br>

### null

null이란 값이 없다는 것(nothing)을 의미한다. 따라서 null로 할당된 변수는 존재하지 않는 값으로 정의되었다고 할 수 있다.

```javascript
var x = null;
console.log( x ); // null 출력
```

null로 할당된 변수는 object 타입을 갖는다.

```javascript
console.log(typeof x); // object
```

<br>

### undefined

만약 변수를 선언 후 값을 할당하지 않으면 그 변수에는 undefined가 할당된다. 

```javascript
var x;
console.log(x); // undefined
console.log( typeof x); // undefined
console.log(y); // ReferenceError
console.log( typeof y); // undefined
```

<br>

### number

자바스크립트는 IEE-754 형식을 사용해 정수 및 부동 소수점을 나타낸다.

```javascript
var x = 10;
var f = 10.5;
var f1 = 0.34;
```

만약 number 값이 부동 소수점으로 나타낸 정수 값이라면 자바스크립트는 이를 부동 소수점이 아닌 정수로 인지한다. 

자바스크립트는 항상 더 적은 메모리를 사용해 계산하는데 부동 소수점 숫자는 정수에 비해 약 두 배의 메모리를 사용하기 때문이다.

```javascript
var fp = 10.00; // 10.00을 정수 10으로 취급
```

<br>

### string

string 타입은 0개 이상의 문자로 이루어진 문자열이다. `'`, `''` 또는 백틱 문자로 시작하고 끝난다.

```javascript
var helloMessage = "Hello";
var message = "let me know";
```

<br>

### boolean

boolean은 true 또는 false를 값을 가진다.

```javascript
var isDay = true;
var isNight = false;
console.log( typeof isDay);// boolean
console.log(typeof isNight);// boolean
```

다른 타입을 boolean으로 변환할 수 있다.

```javascript
console.log(Boolean(‘JavaScript’)); // true console.log(Boolean(‘A’)); // true
console.log(Boolean(‘’)); // false
console.log(Boolean(10)); // true
console.log(Boolean(0)); // false
console.log(Boolean(Infinity)); // true console.log(Boolean({‘A’:1})); //true (빈 객체가 아닐경우 true)
console.log(Boolean(null)); // false
```

<br>

### symbol

symbol은 ES6에서 추가된 원시 타입이다. symbol은 함수를 통해 생성할 수 있으며 호출 될 때마다 고유한 값을 생성한다.

```javascript
console.log(symbol() == symbol() ); // false

const a1 = Symbol(‘debug’);
const a2 = ‘debug’;
const a3 = Symbol(‘xy’);
console.log(a1==a2);// false
console.log(a1==a3); // false
console.log(a1);// Symbol(debug)
```

주로 다른 값과 충돌을 피하기위해 상수 값과 같은 문자열을 만들때 사용된다.

```javascript
const change = Symbol(‘change’);
```

<br>

### bigint

bigint는 숫자 데이터 타입을 Arbitrary-precision arithmetic의 정수형으로 나타낼 수 있는 타입이다.

<br>

### Auto-boxing

원시 타입은 메소드나 프로퍼티를 가지지 않는다. 그러나 아래 코드는 정상적으로 동작한다.

```javascript
const name = "Doggo"
const age = 7

console.log(typeof name) // string
console.log(typeof age) // number

console.log(name.length) // 5
console.log(age.toString()) // "7"
```

자바스크립트에서 원시 타입의  메소드나 프로퍼티에 액세스하려고 할 때마다 원시 타입은 객체(object)로 래핑된다. 이것을 오토박싱이라고 한다. 오토박싱은 원시 타입과 관련된 프로토타입 객체를 원시 타입과 연결해 해당 프로토타입 메소드와 프로퍼티에 액세스 할 수 있게 도와준다.

<br>

<br>

------

**Reference**

- [Primitive, Non-Primitive Data Types in JavaScript | Examples](https://codezup.com/primitive-non-primitive-data-types-in-javascript-examples/#comments)
- [Primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)
- [Do you know what 📦 Autoboxing in JS is?](https://dev.to/benjaminmock/do-you-know-what-autoboxing-in-js-is-enl)