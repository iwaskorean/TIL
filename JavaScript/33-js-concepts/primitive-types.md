# Primitive Types

### 1. Primitive Types

1. **boolean** 
2. **number** 
3. **string** 
4. **null**
   - 존재하지않는 값으로 정의
5. **undefined**
   - 값을 할당하지 않은 변수는 undefined의 값을 가짐
   - 정의가 되지 않았을뿐, 존재하지 않는 것은 아님
6. **symbol**(ES6)



- ECMA Script 표준은 6가지 원시 타입에 더해 객체(object)를 정의, 따라서 원시 타입이 아닌 것은 object

- 원시 타입에는 어떠한 메소드도 붙어있지 않음

- 변수에 원시 타입을 재할당 하는것은 원시타입의 값이 바뀌는 것이 아니라 새로운 원시 타입이 들어감

  > "Primitive types are immutable"

- 원시 타입은 value로 저장되고 객체들은 참조로 저장

  

### 2. Function

- 특별한 프로퍼티들을 가진 새로운 형태의 객체
- 자바스크립트 객체들이 갖는 특성과 같음
  1. 다른 함수의 인자값으로 넘겨질 수 있다.
  2. 변수나 데이터에 할당 가능하다.
  3. 객체의 리턴 값으로 리턴 가능하다.



### 3. Method

- 함수와 같이 객체의 프로퍼티



### 4. Constructor Function

- 리턴값으로 생성하는 함수를 객체 그 자체로서 반환하는 함수

- 어떤 함수든 생성자 함수가 될 수 있음

  ```
  const Foo = function () {};
  const bar = new Foo();
  bar; // {}
  bar instanceof Foo; // true
  bar instanceof Object; // true
  ```

  ```
  const pet = new String("dog");
  // pet은 원시 타입의 "dog" 값을 갖는 것이 아닌 생성자 함수로 생성된 String 객체
  ```

- 일반 함수를 생성자 함수를 실행하면 함수의 역할을 수행하는 것이 아닌 새로운 함수 객체를 반환



### 5. Wrapper Object

- 원시 타입을 new 키워드로 생성하면 원시 타입에 대한 래퍼 객체가 생성

  ```
  const pet = new String("dog");
  typeof pet; // "object"
  pet === "dog"; // false
  // 생성자를 통해 새로운 래퍼 객체 생성
  
  {
  	0: "d",
      1: "o",
      2: "g",
      length: 3
  }
  // 새로운 객체는 "dog"라는 문자열을 위와 같은 프로퍼티로 나타냄
  ```



### 6. Auto-Boxing

```
const pet = new String("dog");
pet.constructor === String; // true
String("dog").constructor === String; // true
// 원시 문자열 타입에서 .constructor 를 이용하여 생성자 프로퍼티를 확인
```

- 원시 타입은 메소드를 가질수 없다고 했는데 어떻게 된것인가?
  - 특정한 원시 타입에서 프로퍼티나 메소드를 호출하려 할 때, 자바스크립트는 처음으로 이것을 임시 래퍼 오브젝트로 바꾼 뒤에 프로퍼티나 메소드에 접근하려고 시도 = 오토 박싱 
- 따라서 자바스크립트 내부에 존재하는 오토 박싱이라는 기능을 통해 몇몇 원시 타입들(String, Number, Boolean)은 객체처럼 동작 할 수 있음



------

**Reference**

- [JavaScript data types and data strutcture](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#primitive_values)
- [자바스크립트의 원시 타입(Primitive Type) (번역)](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-2-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85Primitive-Type-%EB%B2%88%EC%97%AD)