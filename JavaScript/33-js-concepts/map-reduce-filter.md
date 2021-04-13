# map, reduce, filter

<br>

### Why map, filter, reduce?

- 함수형 프로그래밍의 중요한 기반 중 하나는 배열들을 사용하고 배열을 이용한 작업들을  수행하는 것이다.
- 자바스크립트에서 제공하는 map, filter, reduce 메소드를 사용하면 원래의 배열을 그대로 유지하면서 다른 것으로 변환된 배열이나 값을 얻을 수 있다.

<br>

### Map

```
let newArray = arr.map(callback(currentValue[, index[, array]]) {
  // return element for newArray, after executing something
}[, thisArg]);
```

- map은 인자로 콜백을 받으며 이 콜백에는 iteration의 현재 값과 인덱스, 원본 배열이 주어진다.

- 콜백 내부에서 this를 사용하기 위한 두번째 인자(optional)도 있다.

  ```
  const numbers = [1, 2, 3, 4, 5];
  
  const numbersTimesTwo = numbers.map(x => x * 2);
  
  numbersTimesTwo; // [2, 4, 6, 8, 10]
  ```

<br>

### Filter

```
let newArray = arr.filter(callback(currentValue[, index[, array]]) {
  // return element for newArray, if true
}[, thisArg]);
```

- map과 똑같은 인자를 전달받고 비슷하게 동작하지만 다른 점은 콜백이 `true` 또는 `false`로 반환된다는 것이다.

- `true`를 반환할 경우 해당 요소를 계속 가지고 있고 `false`를 반환할 경우 해당 요소를 삭제(filtered out)한다.

  ```
  let duplicateNumbers = [1, 1, 2, 3, 4, 4, 5];
  
  duplicateNumbers.filter((elem, index, arr) => {
  	return arr.indexOf(elem) === index
  	// 요소의 인덱스와 값이 같은 요소 필터링
  }); // [1, 2, 3, 4, 5]
  ```

  ```
  let strings = ["hello", "Matt", "Mastodon", "Cat", "Dog"];
  
  strings.filter(str => {
  	return str.includes("at");
  	// "at" 문자열을 가진 요소 필터링
  })
  // ["Matt", "Cat"]
  ```

<br>

### Reduce

```
arr.reduce(callback(accumulator, currentValue, [, index[, array]] )[, initialValue])
```

- reduce는 하나의 배열을 받아서 하나의 값으로 바꿔준다.

- map, filter와 비슷하지만 콜백 인자에서 다른 점이 있다. 콜백은 모든 반환 값을 누적하는 accumulator, 현재 값, 현재 인덱스, 배열을 받는다.

- 콜백의 최초 호출때 `accumulator`와 `currentvalue `인자는 `initialValue`의 제공 여부에 따라 두 가지 값 중 하나를 가질 수 있다.

  - `initialValue`를 제공한 경우 `accumulator`는 `initialValue`와 같고 `currentValue`는 배열의 첫번째 값과 같다.
  -  `initialValue`를 제공하지 않았다면 `accumulator`는 배열의 첫번째 값과 같고 `currentValue`는 배열의 두 번째 값과 같다.
  
  ```
  let numbers = [1, 2, 3, 4, 5];

  numbers.reduce((acc, curValue) => acc + curValue); // 15
  
  ```
  
  ```
  // 중복되는 값을 가진 요소의 갯수 계산
  let arr = ["a", "b", "a", "c", "c", "c"];
  
  arr.reduce((pre, cur, index, array) => {
  	pre[cur] = ++pre[cur] || 1;
  	return pre;
  }, {});
  
  // {a:2, b:1, c:3}
  ```
  
  ```
  // 중첩된 배열 펼치기
  let arr = [[0, 1], [2, 3], [4, 5]];
  
  arr.reduce(
    ( accumulator, currentValue ) => accumulator.concat(currentValue),
    []
  );
  
  // [0, 1, 2, 3, 4, 5]
  ```
  
  ```
  // 중복 요소 제거
  let arr = [1, 2, 1, 2, 3, 5, 4, 5, 3, 4, 4, 4, 4];
  let result = arr.sort().reduce((accumulator, current) => {
      const length = accumulator.length
      if (length === 0 || accumulator[length - 1] !== current) {
          accumulator.push(current);
      }
      return accumulator;
  }, []);
  console.log(result); //[1,2,3,4,5]
  ```

<br>

### Why use map, filter and reduce?

- array[index] 보다 간단한 형식으로 바로 현재 값에 접근 할 수 있다.
- 기존 배열을 변형을 방지함으로써 예상치 못한 결과(side-effetct)를 최소화할 수 있다.
- for loop를 관리할 필요가 없다.
- 빈 배열들을 만들고 빈 배열들에 push 하지 않아도 된다.

<br>

<br>


------

**Reference**

- [Learn map, filter and reduce in Javascript](https://medium.com/@joomiguelcunha/learn-map-filter-and-reduce-in-javascript-ea59009593c4)
- [JavaScript: Map, Filter, Reduce](https://wsvincent.com/functional-javascript-map-filter-reduce/)
- [map, filter, reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference)