# "?" and "!" in TypeScript

<br>

### `?` : Optional Parameters

- 파라미터에 `?` 접미사를 사용해 특정 타입으로 annotation 할 경우, 그 파라미터는 실제로 특정 타입과 `undefined` 타입을 가지게 된다.

- 옵셔널 파라미터는 사용되지 않을경우(unused, unspecified) `undefined` 값을 갖는다.

  - 값이 있을수도 있고 없을수도 있을때 사용

  ```
  function point(x?: number, y?: number){
      if(x) 
          console.log('X : ' + x);
      else 
          console.log('Value of X coordinate not given');
      if(y) 
          console.log('Y : ' + y);
      else 
          console.log('Value of Y coordinate not given');
  }
  ```

  - OUTPUT

    ```
    // Function Call: point();
    Value of X coordinate not given
    Value of Y coordinate not given
    
    // Function Call : point(10);
    X : 10
    Value of Y coordinate not given
    
    // Function Call : point(10, 20);
    X : 10
    Y : 20
    ```

<br>

<br>

### `!` : Nullable Types

- `!` 를 이용해 annotation된 파라미터는 타입 check시 `null`과 `undefined`를 허용한다.

  ```
  const v: { p: string | null} = { p: 'text' };
  
  console.log(v.p.toUpperCase()); // Error!
  
  console.log(v.p!.toUpperCase()); // Fine
  ```

<br>

<br>

## 참고

<br>

#### `??` : Nullish Coalescing Operator

- `??`는 `||` 연산자와 비슷한 기능으로 `null` 또는 `undefined` 값인지 확인하는 연산자

- 연산자의 왼쪽 변수가 `null`또는 `undfined`일 경우 연산자 우측의 값 반환한다.

  ```
  const undefinedVar = undefined; 
  const value = 'test'; 
  const result = undefinedVar ?? value; 
  
  // result === 'test';
  ```

<br>

#### `!!` 

- `!!`은 자바스크립트에서 연산자로 정의된 것은 아니며 변수를 `boolean` 타입으로 형변환하는 역할을 한다.

- 값이 할당되어 있으면 `true`를 반환, `0`, `null`, `undefined`의 값을 가질 경우 `false`를 반환한다.

  ```
  const array = [1,2];
  !!array[0]; // true
  !!array[1]; // true
  !!array[2]; // false, undefined
  
  const obj = {
    value: 5,
    node: null
  };
  !!obj.value; // true
  !!obj.node;  // false, null
  
  let v = 0;
  !!v // false, 0
  ```

<br>

<br>

------

##### Reference

- [TypeScript 공식 문서](https://www.typescriptlang.org/)
- [JS Double Bang — or “The Not Operator Part !!”](https://medium.com/@edplatomail/js-double-bang-or-the-not-operator-part-40e55d089bf0)