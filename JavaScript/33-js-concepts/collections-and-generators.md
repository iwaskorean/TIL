# Collections and Generators

<br>

### Collections

- 자바스크립트에서 ES5까지는 데이터 컬렉션 타입은 `Array`와 `Object`가 유일했으나 ES6에서 `Map`, `Set`, `WeakSet`, `WeakMap`이 추가되었다.

- `Object`의 문제점

  - 객체의 프로퍼티 key는 항상 문자열(`String`)이어야 한다.(ES6에서는 `Symbol`도 가능)

  - 객체는 본질적으로 반복할 수 없으며, 반복시 많은 비용이 요구된다.

  - 객체에 기본적으로 내장되어있는  `toString`, `constructor`, `valueOf`와 같은 내장 메소드들이 프로퍼티로 추가될 경우 충돌이 발생할 수 있다.

    <br>

1. #### Map

   - `Map`은 key-value의 쌍으로 이루어진 컬렉션이다. 

   ```javascript
   const map = new Map(); // Map 생성
   map.set('hobby', 'cycling'); // map의 key, value 세팅
   
   const foods = { dinner: 'Curry', lunch: 'Sandwich', breakfast: 'Eggs' }; 
   const normalfoods = {}; 
   
   map.set(normalfoods, foods); // 위의 두 객체를 key value 쌍으로 세팅
   
   for (const [key, value] of map) {
     console.log(`${key} = ${value}`); 
     // hobby = cycling  
     // [object Object] = [object Object]
   }
   
   map.forEach((value, key) => {
     console.log(`${key} = ${value}`);
   }, map); 
   // hobby = cycling  
   // [object Object] = [object Object]
   
   console.log(map);
   // Map(2) {"hobby" => "cycling", {…} => {…}}
   	[[Entries]]
   	0: {"hobby" => "cycling"}
   	1: {Object => Object}
   	size: (...)
   	__proto__: Map
   ```

   <br>

2. #### Set

   - `Set`은 중복된 값이 없는 항목들로 이루어진 list이다.

   - 배열과 같이 인덱스가 아닌 값을 key로 사용한다.

   ```javascript
   const mySet = new Set();
   
   mySet.add(1).add(5); // Set { 1, 5 }, 체이닝이 가능하다.
   mySet.add(5); // Set { 1, 5 }, 중복된 값을 add 할 경우 추가되지 않는다.
   mySet.add('some text'); // Set { 1, 5, 'some text' }
   
   const o = {a: 1, b: 2};
   mySet.add(o);
   
   mySet.add({a: 1, b: 2}); // 객체 o와 다른 객체를 참조하므로 새로운 값이 추가 된다.
   
   mySet.size; // 5
   
   mySet.has(1); // true
   mySet.has(3); // false, 3은 set에 추가되지 않았음
   mySet.has(5);              // true
   mySet.has(Math.sqrt(25));  // true
   mySet.has('Some Text'.toLowerCase()); // true
   mySet.has(o); // true
   
   mySet.delete(5); // set에서 5를 제거함
   mySet.has(5);    // false, 5가 제거되었음
   ```

   <br>

3. #### Weak Collection

   - `Map`과 `Set`의 객체에 대한 참조는 가비지 컬렉션을 허용하지 않는다. 따라서 크기가 큰 `Map`과 `Set`의 객체가 더이상 필요하지 않는다면 이를 처리하기위해 큰 비용이 필요하다.

   - 이러한 문제점을 해결하기 위한 것이 `WeakMap`과 `WeakSet`이며 참조하는 객체가 더이상 사용되지 않을 때 가비지 컬렉션의 처리를 허용한다.

   ```javascript
   // WeakMap
   
   const aboutAuthor = new WeakMap(); // WeakMap 생성
   const currentAge = {}; // key는 반드시 객체여야 한다
   const currentCity = {}; 
   
   aboutAuthor.set(currentAge, 30); 
   aboutAuthor.set(currentCity, 'Denver'); // value 타입의 제한은 없다
   
   console.log(aboutAuthor.has(currentCity)); // true
   
   aboutAuthor.delete(currentAge); // Delete a key
   ```

   ```javascript
   // WeakSet
   
   var ws = new WeakSet();
   var obj = {};
   var foo = {};
   
   ws.add(window);
   ws.add(obj);
   
   ws.has(window); // true
   ws.has(foo);    // false, foo가 집합에 추가되지 않았음
   
   ws.delete(window); // 집합에서 window 제거함
   ws.has(window);    // false, window가 제거되었음
   ```

<br>

### Generator

- 일반적인 함수는 리턴할때 오직 하나의 값을 리턴하지만 제너레이터를 사용하면 여러 값을 하나씩 반환 할 수 있다.

- 제너레이터를 생성하기 위해서는 `function*`라는 제너레이터 함수가 필요하다.

- 제너레이터 함수를 호출하면 코드가 실행되지 않고 대신에 실행을 처리하는 "제너레이터 객체"라는  특별한 객체가 리턴된다.

  ```javascript
  function* generateSequence() {
    yield 1;
    yield 2;
    return 3;
  }
  
  // 제너레이터 함수는 '제너레이터 객체'를 생성한다.
  let generator = generateSequence();
  alert(generator); // [object Generator]
  ```

- `next()`는 제너레이터의 주요 메소드이며 `next()`를 호출하면 가장 가까운 `yield value` 를 만날때까지 실행이 지속된다.

- `next()`는 항상 `value`와 `done`(코드 실행이 끝났을 경우 `true`, 아니면 `false`) 두 프로퍼티를 가진 객체를 리턴한다.

  ```javascript
  function* generateSequence() {
    yield 1;
    yield 2;
    return 3;
  }
  
  let generator = generateSequence();
  
  let one = generator.next();
  alert(JSON.stringify(one)); // {value: 1, done: false}
  
  let two = generator.next();
  alert(JSON.stringify(two)); // {value: 1, done: false}
  
  let three = generator.next();
  alert(JSON.stringify(three)); // {value: 1, done: true}
  ```

- 제너레이터는 iterable 객체이므로 `for..of` 나 `...` 같은 문법을 사용할 수 있다.

  ```javascript
  function* generateSequence() {
    yield 1;
    yield 2;
    yield 3;
  }
  
  let generator = generateSequence();
  
  for(let value of generator) {
    alert(value); // 1, 2, 3
  }
  
  let sequence = [0, ...generateSequence()];
  
  alert(sequence); // 0, 1, 2, 3
  ```

<br>

<br>

------

**Reference**

- [ES6 Collections: Using Map, Set, WeakMap, WeakSet](https://www.sitepoint.com/es6-collections-map-set-weakmap-weakset/)
- [Set](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [Generators](https://javascript.info/generators)

