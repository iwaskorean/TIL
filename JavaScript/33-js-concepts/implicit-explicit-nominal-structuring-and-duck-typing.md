# Implicit / Explicit Coercion and Duck Typing



## Implicit Type Coercion 

암시적 변환이란 자바스크립트가 예상하지 못한(unexpected) 값 타입을 예상되는 값 타입으로 강제 변환하려는 시도이다. 즉, 암시적 변환은 자바스크립트에 의해 자동으로 일어나는 변환이다.

예를 들어 number 타입으로 예상되는 곳에 string 타입의 값을 넣을 경우 string을 number 타입으로 바꾸려고 시도한다.

<br>

### Non-numeric values in numeric expression

#### String

숫자를 이용한 식에서 string을 피연산자로 전달할때, 숫자만 포함된 문자열일 경우 string을 number 타입으로 변환되며 숫자가 아닌 문자열은 NaN으로 변환된다.

```javascript
3 * "3" // 3 * 3
3 * Number("3") // 3 * 3
Number("5") // 5

Number("1.") // 1
Number("1.34") // 1.34
Number("0") // 0
Number("012") // 12

Number("1,") // NaN
Number("1+1") // NaN
Number("1a") // NaN
Number("one") // NaN
Number("text") // NaN
```

<br>

#### `+` operation

`+` 연산자는 수학적인 개념의 추가 또는 문자열의 연결의 기능을 가진다. `+` 연산자의 피연산자가 string일때 자바스크립트는 number를 string 타입으로 변환한다.

```javascript
1 + "2" // "12"
1 + "js" // "1js"

1 + 2 // 3
1 + 2 + 1 // 4

1 + 2 + "1" // "31"
(1 + 2) + "1" // "31"

1 + "2" + 1 // "121"
(1 + "2") + 1 // "121"
```

<br>

### Objects

대부분의 자바스크립트 객체의 변환은 [objects Object]의 결과값을 가진다.

```javascript
"name" + {} // "name[object Object]

const foo = {}
foo.toString() // [object Object]

const baz = {
  toString: () => "I'm object baz"
}

baz + "!" // "I'm object baz!"
```

모든 자바스크립트 객체들은 `toString` 메소드를 상속받는다. `toString` 메소드는  객체를 문자열로 변환하기 위해 호출된다.

`toString` 메소드의 결과값은 문자열 연결 및 numeric expression에서 사용할 수 있다.

```javascript
const foo = {
  toString: () => 4
}

2 * foo // 8
2 / foo // 0.5
2 + foo // 6
"four" + foo // "four4"

const baz = {
  toString: () => "four"
}

2 * baz // NaN
2 + baz // 2four

const bar = {
  toString: () => "2"
}

2 + bar // "22"
2 * bar // 4
```

<br>

### Array objects

배열 객체에서의 `toString  ` 메소드는 `join` 메소드가 인수없이 호출될때의 동작과 유사하게 작동한다.

```javascript
[1,2,3].toString() // "1,2,3"
[1,2,3].join() // "1,2,3"
[].toString() // ""
[].join() // ""

"me" + [1,2,3] // "me1,2,3"
4 + [1,2,3] // "41,2,3"
4 * [1,2,3] // NaN
```

<br>

### True or False

```javascript
Number(true) // 1
Number(false) // 0
Number("") // 0

4 + true // 5
3 * false // 0
3 * "" // 0
3 + "" // "3"
```

<br>

### `valueOf` method

`valueOf` 메소드는 숫자 값을 나타내기 위한 메소드이다. string 또는 number 타입으로 예상되는 값에서 `valueOf` 메소드를 정의 할 수 있다.

`toString`과 `valueOf` 메소드가 같이 있는 경우 `valueOf` 메소드가 우선순위를 갖는다.

```javascript
const foo = {
  valueOf: () => 3
}
3 + foo // 6
3 * foo // 9

const bar = {
  toString: () => 2,
  valueOf: () => 5
}

"sa" + bar // "sa5"
3 * bar // 15
2 + bar // 7

```

<br>

### False and Truthy

자바스크립트의 모든 변수들은 `true` 또는 `false`로 표현될 수 있다.

`false`값으로 표현되는 값들은 아래와 같다.

- false
- 0
- null
- undefined
- ""
- NaN
- -0

위 값들을 제외하고는 모두 `true`로 표현된다.

<br>

<br>

## Explicit Type Coericion 

Type casting과 같은 의미이며 개발자가 의도를 가지고 변환할 타입을 명시적으로 변환하는 것이다.

```javascript
Number('123'); // 123
String(123); // '123'
Boolean(3); // true
	...
```

<br>

<br>

## Duck Typing

> 만약 어떤 것이 오리처럼 생겼고, 오리처럼 헤엄치고, 오리처럼 꽥꽥되면, 아마 그것은 오리일 것이다.

덕 타이핑이란 클래스 상속이나 인터페이스로 타입을 구분하는 것이 아니라 객체의 변수 및 메소드의 집합이 객체의 타입을 결정하는 동적 타이핑의 한 종류이다.

<br>

<br>


------

**Reference**

- [What you need to know about Javascript's Implicit Coercion](https://dev.to/promhize/what-you-need-to-know-about-javascripts-implicit-coercion-e23) 
- [JavaScript type coercion explained](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/) 
- [Duck typing in javascript](https://stackoverflow.com/questions/3379529/duck-typing-in-javascript)