# Literal Type and Template Literal Type

<br>

### Literal Type

- 리터럴 타입인 `string`과 `number ` 타입을 이용해 특정 문자열이나 숫자를 타입으로 지정

  ```
  let changingString = "Hello World";
  changingString = "Ola Mundo";
  // let으로 선언된 changingString은 다른 string값으로 값이 변경될 가능성이 있으므로 string으로 inference
  // ^ = let changingString: string
  
  const constantString = "Hello World";
  // const로 선언된 constantString은 값이 변경될 수 없으므로 리터럴 타입으로 inference
  // ^ = const constantString: "Hello World"
  ```

  <br>

- 리터럴 타입과 유니언을 결합한 형태의 사용

  ```
  function textAlignment(s: string, alignment: "left" | "right" | "center") {
    // ...
  }
  textAlignment("Hello, world", "left");
  textAlignment("G'day, mate", "centre"); // *1, error
  
  // 두번째 인자는 "left" 또는 "right" 또는 "center"로 타입 annotate 됐기 때문에 *1에서 error
  ```

  ```
  interface Options {
    height: number;
  }
  function set(x: Options | "auto") {
    // ...
  }
  set({ height: 100 });
  set("auto");
  set("longer"); // error
  ```

<br>

- #### Literal Inference

  - `object`로 변수를 초기화 할때 Type system에서는 해당 객체의 프로퍼티가 나중에 값을 바꿀수 있다고 판단

    ```
    const obj = { amount: 0 };
    if (someCondition) {
      obj.amount = 1;
    }
    
    // 객체 내의 amount를 number 리터럴 타입 0으로 annotate을 했지만 객체이므로 나중에 값이 바뀌어질 수 있기때문에 number로 inference됨
    ```

    <br>

    <br>

### Template Literal Type

- 템플릿 리터럴 타입은 `string` 리터럴 타입을 기반으로 유니언을 통해 더 많은 문자열로 확장

- 자바스크립트 ES6의 템플릿 문자열과 같은 문법을 가지지만, type의 위치에서 사용

  ```
  type World = "world";
  
  type Greeting = `hi ${World}`;
  //   Greeting의 타입은 'hello world'
  ```

  ```
  type Apple = "red" | "sweet";
  type Lemon = "Yello" | "sour";
  
  type Fruit = `I_like_${Apple | Lemon}_fruit!`;
  //  Fruit의 타입은  = "red" | "sweet" | "Yello" | "sour"
  ```

  

<br>

<br>

------

##### Reference

- [TypeScript 공식 문서](https://www.typescriptlang.org/)

