# ==  vs  ===  vs  typeof



### Triple Equals(`===`)

- 엄격한(strict) 동등성 확인

- 타입과 값(value) 모두 동일해야한다.

  ```
  5 === 5 // true, 좌변 우변 모두 number 타입이며 값 또한 5로 동일
  
  'hello world' === 'hello world' // true, string 타입이며 값 또한 동일
  
  true === true // true 
  ```

  ```
  77 === '77' // false
  
  false === 0 // false, 타입과 값 모두 다름
  ```





### Double Equals(`==`)

- 느슨한(loose) 동등성 확인

- 타입 강제 변환(type coercion conversion)을 수행

  ```
  77 === '77' // false
  
  77 == '77' // true
  // string 타입의 '77'이 number 77로 type coericion
  ```

  ```
  false === 0 // false 
  
  false == 0 // true
  // 0은 거짓 값(falsy value)를 가지므로 '=='에서는 true
  ```

  - Falsy values : `false`, `0`, `""`(empty string), `null`, `undefined` ,`NaN`

    ```
    false == 0 && 0 == "" && "" == false // true
    
    null == null && undefined == undefined && null == undefined // true
    
    NaN == null && NaN == undefined && NaN == NaN
     // false, NaN은 어떤 값과도 동일하지 않음
    ```



### typeof

- 피연산자의 타입을 반환

- 선언되지 않은 변수를 피연산자로 가지고 있을경우 `undefined`

- `string`, `number`, `string`, `boolean`, `object`, `function`, `symbol`

  ```
  typeof 3 // "number"
  typeof "abc" // "string"
  typeof {} // "object"
  typeof true // "boolean"
  typeof undefined // "undefined"
  typeof function(){} // "function"
  ```

- `object`

  - `Date`, `정규표현식`, `사용자 정의 객체`, `DOM요소`, `null` 등 위의 7가지를 제외한 대부분의 객체를 `object`로 반환

  ```
  typeof [] // "object"
  typeof null // "object"
  ```






------

**Reference**

- [JavaScript — Double Equals vs. Triple Equals](https://codeburst.io/javascript-double-equals-vs-triple-equals-61d4ce5a121a)

- [Checking Types in Javascript](http://tobyho.com/2011/01/28/checking-types-in-javascript/)

  