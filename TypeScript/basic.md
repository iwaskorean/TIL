# Basic

>   *Udemy 강좌 React and Typescript: Build a Portfolio Project(by Stephen Grider)의 Appendix: Typescript 섹션 을 요약한 포스트입니다.*

<br>

### Typescript

- 자바스크립트에 Type system을 적용
- Type system
  - 에러를 catch 하는 것을 도와줌
  - Type annotation을 통해 프로퍼티들에 대한 묘사와 코멘트 제공
  - 코드 작성시에만 active
- Process
  - TypeScript code → TypeScript complier→ Plain old JavaScript
  - 브라우저에서는 타입스크립트가 아닌 자바스크립트로 execute

<br>

### Type

- 프로퍼티와 값들을 가지고 있는 함수를 나타내기 위한 방식

- Primitive Types 

  -  `number`, `boolean`, `void`, `undefined`, `string`, `symbol`, `null`

- Object Types

  - `functions`, `arrays`, `classes`, `objects`

  <br>

- ##### 왜 타입에 신경써야하는가?

  > Why do we care about types?
  - 타입들은 타입스크립트 컴파일러가 에러를 위해 우리가 작성한 코드를 분석할때 사용되기 때문에

  - 다른 사람이 작성한 코드의 이해를 돕기 위해서

    <br>

- ##### 어디에서 타입을 사용해야 하는가?

  > Where do we use type?

  - Everywhere

<br>

<br>

## Type Inference and Type Annotation

<br>

### Type Inference

> Typescript tries to figure out what type of value a variable refers to

- 타입스크립트가 자동으로 타입을 추측 

- ##### When to use ..

  - Always!

<br>

### Type Annotation

> Code we add to tell Typescript what type of value a variable will refer to

- 우리가 직접 명시적으로 값이 가르키는 타입을 지정

- ##### When to use ..

  - 변수를 선언하고 다른 라인에서 초기화할때
  - Type inference로 정확한 타입이 주어지지 않을때
  - `any` 타입을 반환하는 함수
  - 값을 명확하게 할 필요가 있을때

  ```
  const num1: number = 5; // number type으로 type annotation
  const str: string = 'Hi';
  
  let employee : { 
      id: number; 
      name: string; 
  }; 
  employee = { 
    id: 100, 
    name : "John"
  }
  ```

  ```
  const json = `{"x": 10, "y": 20}`;
  
  // type annotation을 하지 않았을때
  const coord = JSON.parse(json);
  coord.abc; // undefined, coord가 abc라는 프로퍼티를 가지고 있지않으므로 undefiend
  
  // type annotation을 했을때
  const coordTyped: {x:number; y:number} = JSON.parse(json);
  coord.abc;  // error
  ```

  ```
  // Type inference로 정확한 타입이 주어지지 않아 error
  let numbers = [-10, -1, 12];
  let numberAboveZero = false;
  
  for(let i = 0 ; i < numbers.length; i++) {
  	if(numbers[i] > 0) {
  		numberAboveZero = numbers[i]; 
  		// error, type inference로 정확한 타입을 알 수 없음.
  	}
  }
  
  // Type annotation
  let numbers = [-10, -1, 12];
  let numberAboveZero: boolean | number = false;
  
  for(let i = 0 ; i < numbers.length; i++) {
  	if(numbers[i] > 0) {
  		numberAboveZero = numbers[i]; 
  		// error, type inference로 정확한 타입을 알 수 없음.
  	}
  }
  ```

  ```
  // 비구조적(unstructured) annotation
  const profile = {
  	name:'alex',
  	age: 20
  }
  
  const { age }: {age: number} = profile;
  ```

<br>

<br>

## 기타 내용들 정리

### Types

- tuple

  - 배열과 유사하지만 각 엘리먼트들의 타입을 명시 할 수 있음

  ```
  let x: [string, number];
  x = ["hello", 2];
  ```

- enum

  - 숫자 데이터셋에 이름을 부여

    ```
    enum Color {
      Red = 1,
      Green,
      Blue,
    }
    let c: Color = Color.Green;
    ```

- any

  - 알지못하는 변수 유형을 설명
  - 컴파일시 타입검사는 하지 않는다.

- never

  - 절대로 발생하지 않는 값의 타입 

  - 항상 예외를 발생시키거나 반환하지않는 함수 표현식이나 화살표 함수의 반환 유형

    ```
    // Function returning never must not have a reachable end point
    function error(message: string): never {
      throw new Error(message);
    }
    
    // Inferred return type is never
    function fail() {
      return error("Something failed");
    }
    
    // Function returning never must not have a reachable end point
    function infiniteLoop(): never {
      while (true) {}
    }
    ```

<br>

### Type Array

- 각 요소가 특정한 타입의 값인 배열

- ##### 왜 사용해야하는가?

  - 다른 배열 요소들의 타입과 맞지않는(imcompatible) 값이 배열에 추가되는것을 막을 수 있다.
  - `map`, `forEach`, `reduce` 함수들을 사용할때 도움을 준다.
  - 배열에서 요소가 extracting 될때 type inference를 할 수 있게 된다.

  ```
  const arr: string[][] = []; //2차원의 string 배열로 annotate
  
  arr.push(100); // error
  ```

<br>

<br>

### Interface

- 객체의 프로퍼티 이름과 값 타입을 묘사하는 하나의 타입

  ```
  interface Vehicle {
  	name: string;
  	broken: boolean;
  }
  
  const oldCivic = {
  	name: 'civic',
  	broken:1
  }
  
  const printVehicle = (vehicle: Vehicle): void => {
  	console.log(`Name : ${vehicle.name}`);
      console.log(`Broken? : ${vehicle.broken}`);
  }
  
  printVehicle(oldCivic); // error,
  // Vehicle interface에서 broken을 boolean 타입으로 annotate 했으므로 
  // broken이 number 타입으로 init된 oldCivic은 타입 검사에서 걸리게 됨.
  ```

<br>

<br>

<br>

------

##### Reference

[TypeScript  공식 문서](https://www.typescriptlang.org/)

