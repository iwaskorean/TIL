# ==  vs  ===  vs  typeof

### `===` : Triple Equals

`===`는 엄격한(strict) 동등성을 확인하는 연산자이다. 타입과 값 모두 동일하면 `true`를 반환하며 그렇지 않을 경우 `false`를 반환한다.

```javascript
5 === 5 // true, 좌변 우변 모두 number 타입이며 값 또한 5로 동일

'hello world' === 'hello world' // true, string 타입이며 값 또한 동일

true === true // true 
```

```javascript
77 === '77' // false

false === 0 // false
```

<br>

### Double Equals(`==`)

`==`는 느슨한(loose) 동등성을 확인하는 연산자이며 자동으로 타입 강제 변환(type coercion conversion)을 수행한다.

```javascript
77 === '77' // false
77 == '77' // true
// string '77'이 number 77로 type coericion
```

```javascript
false === 0 // false 
false == 0 // true
// 0은 거짓 값(falsy value)를 가지므로 '=='에서는 true
```

`0` 이외에도 `false`, `""`(빈 문자열), `null`, `undefined`, `NaN`은 거짓 값(falsy value)를 가진다.

```javascript
false == 0 && 0 == "" && "" == false // true

null == null && undefined == undefined && null == undefined // true

NaN == null && NaN == undefined && NaN == NaN
 // false, NaN은 어떤 값과도 동일하지 않음

Boolean(undefined) // false
Boolean(null) // false
```

<br>

### typeof

`typeof`는 피연산자의 타입을 반환하는 연산자이다. 피연산자가 선언되지않은 변수일 경우 `undefined`를 반환한다.

```javascript
typeof 3 // "number"
typeof "abc" // "string"	
typeof {} // "object"
typeof true // "boolean"
typeof undefined // "undefined"
typeof function(){} // "function"
```

`Date`, 정규표현식, 사용자 정의 객체, DOM요소, `null` 등은 `object`로 반환된다.

```javascript
typeof [] // "object"
typeof null // "object"

const d = new Date()
typeof d // "object"

const regExp = /^[a-z]/
typeof regExp // "object"
```

<br>

<br>


------

**Reference**

- [JavaScript — Double Equals vs. Triple Equals](https://codeburst.io/javascript-double-equals-vs-triple-equals-61d4ce5a121a)
- [Checking Types in Javascript](http://tobyho.com/2011/01/28/checking-types-in-javascript/)

  