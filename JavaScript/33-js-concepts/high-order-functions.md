# High Order Functions

### Introducting HoF

- 자바스크립트에서 함수는 1급 객체(first class object)이다.

  - 1급 객체(first class object) : 객체(obejct)를 1급 시민(first class citizen)으로써 취급

  - 1급 시민(first class citizen)의 조건

    - 변수(variable)에 담을 수 있다.

    - 인자(parameter)로 전달할 수 있다.

    - 반환값(return value)으로 전달할 수 있다.
    


- 따라서 함수형 프로그래밍 패턴인 고차 함수(HoF)가 가능하며, 고차 함수는 다른 함수를 인자로 받거나 리턴 할 수 있는 함수를 말한다.

- 고차함수를 사용하면 코드를 논리적으로 표현(composability)하고 가독성을 높일 수 있다.

<br>

<br>

## Examples

### Filtering Collections

- 배열에서 짝수 값을 가진 원소만 가져오기

  ```javascript
  const nums = [1, 2, 3, 6, 8, 11];
  
  function filter(nums, test) {
        let result = [];
        for(let i=0; i<nums.length; i++) {
            if(test(nums[i])) {
                result.push(nums[i])
            }
        }
        return result;
    }
  
  let result = filter(nums, num => num % 2 == 0);
  result;      // [2, 6, 8]
  ```

  - 첫번째 인자인 `nums`는 배열을 가져오고 두 번째 인자 `test`를 통해 필터링을 실행해 새로운 배열인 `result`를 반환한다.

<br>

### Grouping

- 태그를 설정해 객체를 그룹화

  ```java
  function group(items, groupBy) {
          let grouped = Object.create(null);
  
          for(let i=0; i < items.length; i++) {
              let tag = groupBy(items[i])
              if(tag in grouped) {
                  grouped[tag].push(items[i])
                  continue;
              }
              grouped[tag] = [items[i]];
  
          }
  
          return grouped;
      }
  
  const items = [
       {tag: "car", name: "tesla", model: "Y"},
       {tag: "smartphone", name: "Samsung", yr: "2019"},
       {tag: "car", name: "mercedes", model: "classic"},
       {tag: "gaming", name: "PS5"},
       {tag: "smartphone", name: "Iphone", yr: "2019"}
  ]
  const tagged = group(items, item => item["tag"]);
  
  tagged
  /*
    {
       car: [
          { tag: 'car', name: 'tesla',model: "Y"},
          { tag: 'car', name: 'mercedes', model: "classic" }
       ],
       smartphone: [
          { tag:'smartphone', name: 'Samsung s9', yr: "2018" },
          { tag:'smartphone', name: 'Iphone 11', yr: "2019" }
       ],
       gaming: [ { tag: 'gaming', name: 'PS5' } ]
      }
  */
  ```

<br>

### Flattening Arrays

- 배열 내부에 중첩된(nested) 배열을 평평하게(flat) 만들기

  ```javascript
  const nest = [1, 2, [3, 5], 0];
  const deeper = [1, 2, [3, 5, [0, 9, 1]], 0];
  
  function flatten(nested) {
      return nested.reduce((flat, next) => {
  
          if(Array.isArray(next)) {
              return [...flat, ...flatten(next)]
          }
  
          return [...flat, next]
  
      }, [])
  };
  
  flatten(nest); // [1, 2, 3, 5, 0]
  flatten(deeper); // [1, 2, 3, 5, 0, 9, 1, 0]
  ```

<br>

### Built-in in Array Object

- `Array` 객체에 내장된 메소드들 또한 고차함수로써 동작한다.

  ```javascript
  // #1
  const numbers = [1, 2, 3];
  
  numbers
   .map((n) => n * 2) // it will return [2, 4, 6]
   .filter((n) => n % 4) // it will filter out number that divides by 4
   .reduce((a, b) => a + b); // return 6 - sum of the array items
  ```

  ```javascript
  // #2
  function compose(...fns) {
      return function(arr) {
          return fns.reduceRight((acc, fn) => fn(acc), arr);
      }
  } // 
  
  function pow2(arr) {
      return arr.map(v => v * v)
  }
  
  function filterEven(arr) {
      return arr.filter(v => v % 2);
  }
  
  const pipe = compose(filterEven, pow2);
  
  pipe([1, 2, 3, 4]) // [1, 9];
  ```

  - 다수의 콜백을 전달해 각각 다른 파이프라인을 만들고 다양한 데이터를 전달할 수 있다.

<br>

<br>

###### Note 
###### Built-in in Array Object #2

------

**Reference**

- [Higher Order Functions - A pragmatic approach](https://dev.to/nuel_ikwuoma/higher-order-functions-a-pragmatic-approach-51fb)
- [Higher-Order Functions In JavaScript](https://dev.to/spukas/higher-order-functions-in-javascript-1f4n)
- [JavaScript의 함수는 1급 객체(first class object)이다](https://bestalign.github.io/2015/10/18/first-class-object/)