# new, Constructor

- 유사한 객체를 다수 만들어야할때 `new` 연산자와 생성자 함수를 사용하면 많은 수의 유사한 객체들을 만들 수 있다.

<br>

### Constructor function

- 재사용 가능한 객체 생성을 위해 `new` 연산자로 실행하는 함수를 생성자 함수라고 한다.

- 모든 함수는 생성자 함수가 될 수 있다. 그러나 생성자 함수는 관례적으로 일반 함수와 구분하기 위해 함수 이름의 첫 글자를 대문자로 지정한다.

- `new` 연산자를 사용해  함수를 실행할때 아래와 같은 동작이 일어난다.

  1. 빈 객체가 생성되어 this에 할당된다.

  2. 함수 body의 코드들이 실행되고 `this`에 새로운 프로퍼티를 추가해 `this`를 수정한다.

  3.  `this`의 값을 리턴한다.

     ```
     function User(name) {
       // this = {};  // this가 암시적으로 생성된 후 빈 객체 할당
     
       // 빈 객체가 할당된 this에 프로퍼티들을 더해 this를 modify
       this.name = name;
       this.isAdmin = false;
     
       // return this;  (implicitly)
     }
     ```

  <br>

### Return from constructor

- 일반적으로, 생성자 함수는 반환할때 필요한 모든 것들은 `this`에 저장하고 자동으로 `this`를 리턴하기 때문에 `return`문이 없다. 

- `return`문을 명시적으로 작성할 경우

  - 객체를 리턴할 경우, `this` 대신 객체가 리턴된다.

  - 원시 값(primitive)를 리턴할 경우, `return`문은 무시된다.

    ```
    // 객체를 리턴할 경우
    function BigUser() {
    
      this.name = "John";
    
      return { name: "Godzilla" };  //  객체를 반환하는 return문을 명시적으로 작성
    }
    
    alert( new BigUser().name );  // Godzilla, this가 아닌 명시적으로 작성한 객체가 반환됨
    
    // 원시 값(string value)을 리턴할 경우
    function SmallUser() {
    
      this.name = "John";
    
      return "Ant"; // <-- returns this
    }
    
    alert( new SmallUser().name );  // John, "Ant"를 명시적으로 리턴했지만, 무시하고 this가 반환됨
    ```

  <br>

### Methods in constructor

- 생성자 함수로 객체를 생성하면 객체의 프로퍼티뿐만 아니라 메소드들도 파라미터들을 사용해 유연하게 구성 할 수 있다.

```
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

<br>

<br>

## instanceof and instances

> instance : object가 constrctor로 생성될때, 그 object는 constructor의 instance이다. (def by MDN)

- *object*  `instanceof`  *constructor* 

- object의 프로토타입 체인에 constructor.prototype이 존재하는지 판별해 `true` / `false`를 리턴하는 연산자이다.

  ```
  const valueFirst  = new String(“black”);
  
  valueFirst instanceof String; // true
  
  valueFirst instanceof Object; // true
  
  
  /*
  valueFirst
  	> String {"black"}
  		> __proto__: String
  			> __proto__ : Object
  */
  ```

  ```
  const valueSecond = “black”; // object is not specified 
  
  valueSecond instanceof String; // false, it is not a String.
  
  typeof valueSecond   // string
  
  /*
  valueSecond
  	> "string"
  */
  ```

  ```
  class C {
  ...
  }
  
  const valueThird = new C();
  
  valueThird instanceof C // true
  
  /*
  valueThird
  	> C { ... }
  		> __proto__ : Object
  			> constructor : class C
  */
  
  // class가 아닌 생성자 함수일 경우에도 같은 결과이다.
  ```

<br>

<br>


------

**Reference**

- [Constructor, operator "new"](https://javascript.info/constructor-new)
- [What Is the Instanceof Operator in JavaScript?](https://www.developintelligence.com/blog/2016/10/what-is-the-instanceof-operator-in-javascript/)
- [instanceof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)