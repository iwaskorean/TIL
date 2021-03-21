# Implicit / Explicit Coericion and Duck Typing



### Implicit Type Coericion 

- 자바스크립트가 예상하지 못한(unexpected) 값 타입을 예상되는 값 타입으로 강제 변환 하려는 시도

- 예를 들어, number 타입으로 예상되는 곳에 string 타입의 값을 넣을 경우 string을 number 타입으로 바꾸려고 시도한다.

  

- #### Non-numeric values in numeric expression

  - Strings

    - 숫자를 이용한 식에서 string을 피연산자로 전달할때,

      - 숫자만 포함된 문자열일 경우 string을 number 타입으로 변환

      - 숫자가 아닌 문자열은 NaN으로 변환

        ```
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

  - `+` operation

    - 수학적인 추가 또는 문자열의 연결의 기능을 가진다.

    - `+`연산자의 피연산자가 string일때 자바스크립트는 number를 string 타입으로 변환

      ```
      1 + "2" // "12"
      1 + "js" // "1js"
      
      1 + 2 // 3
      1 + 2 + 1 // 4
      
      1 + 2 + "1" // "31"
      (1 + 2) + "1" // "31"
      
      1 + "2" + 1 // "121"
      (1 + "2") + 1 // "121"
      ```

  - Objects

    - 대부분의 자바스크립트 객체의 변환은 [objects Object]의 결과값을 가진다.

      ```
      "name" + {} // "name[object Object]
      
      const foo = {}
      foo.toString() // [object Object]
      
      const baz = {
        toString: () => "I'm object baz"
      }
      
      baz + "!" // "I'm object baz!"
      ```

    - 모든 자바스크립트 객체들은 `toString` 메소드를 상속받으며 객체가 문자열로 변환 될 때마다 호출 된다.

    - `toString` 메소드의 결과값은 문자열 연결 및 수학 표현식에 이용된다.

  - Array objects

    - 배열 객체에서의 `toString  ` 메소드는 `join` 메소드가 인수없이 호출될때의 동작과 유사하게 작동한다.

      ```
      [1,2,3].toString() // "1,2,3"
      [1,2,3].join() // "1,2,3"
      [].toString() // ""
      [].join() // ""
      
      "me" + [1,2,3] // "me1,2,3"
      4 + [1,2,3] // "41,2,3"
      4 * [1,2,3] // NaN
      ```

  - True or False

    ```
    Number(true) // 1
    Number(false) // 0
    Number("") // 0
    
    4 + true // 5
    3 * false // 0
    3 * "" // 0
    3 + "" // "3"
    ```

  - `valueOf` method

    - `valueOf` 메소드는 숫자값을 나타내기 위한 메소드이다.

    - string 또는 number 타입으로 예상되는 값에서 `valueOf` 메소드를 정의 할 수 있다.

    - `toString`과 `valueOf` 메소드가 같이 있는경우 `valueOf` 메소드가 실행된다

      ```
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

      

- #### False and Truthy

  - 자바스크립트의 모든 변수들은 `true` 또는 `false`로 표현될 수 있다.
  - `false`값으로 표현되는 값들은
    - false
    - 0
    - null
    - undefined
    - ""
    - NaN
    - -0
  - 위 값들을 제외하고는 모두 `true`로 표현된다.



### Explicit Type Coericion 

- Type casting과 같은 의미이다.

- 개발자가 의도를 가지고 변환할 타입을 명시적으로 변환하는 것이다.

  ```
  Number('123');
  String(123);
  Boolean(3);
  	...
  ```



### Duck Typing

> 만약 어떤 것이 오리처럼 생겼고, 오리처럼 헤엄치고, 오리처럼 꽥꽥되면, 아마 그것은 오리일 것이다.

- 클래스 상속이나 인터페이스로 타입을 구분하는 것이 아니라 객체의 변수 및 메소드의 집합이 객체의 타입을 결정
- 동적 타이핑의 한 종류




------

**Reference**

- [What you need to know about Javascript's Implicit Coercion](https://dev.to/promhize/what-you-need-to-know-about-javascripts-implicit-coercion-e23) 
- [JavaScript type coercion explained](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/) 
- [Duck typing in javascript](https://stackoverflow.com/questions/3379529/duck-typing-in-javascript)