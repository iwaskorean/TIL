

# Partial Applications, Currying, Compose and Pipe

파셜 어플리케이션과 커링은 함수형 프로그래밍 컨셉들 중 하나이다. 자바스크립트에서 함수는 일급 시민(first-class citizen)이므로 파셜 어플리케이션과 커링이 가능하다.

<br>

### Partial application

파셜 어플리케이션은 일부 인자에 함수를 적용하기 위한 프로세스이다. 함수와 다수의 인자를 전달받아 더 적은 수의 인자로 그 함수를 실행할 수 있는 함수를 만든다. 즉, 여러개의 인자를 받는 함수에서 일부 인자를 고정하는 것이다.

```javascript
const adder = (a, b) => a + b;
const add10 = partial(adder, 10);

add10(20); // 30

/*

add10(20)을 풀어서 작성하면

function partiallyApplied(...laterArgs) {
  return adder(10, ...laterArgs);
}(20);

*/
```

- `partial`에 의해 만들어진 `partiallyApplied()` 함수에 고정되지 않은 나머지 인자(`...laterArgs`)를 전달해서 `adder()` 함수를 호출하는 과정이다.

<br>

### Currying

커링은 파셜 어플리케이션의 특수한 형태이며 다수의 인자가 아닌 인자를 하나씩 고정한다.  그리고 원래의 함수가 요구하는 인자가 모두 전달될 때까지 재귀적으로 curry 함수를 생성한다.

```javascript
// 일반적인 함수 호출
const add = (a, b) => a + b
add(1, 2) // 3

// 커링 적용
const add = a => b => a + b
add(1)(2) // 3
```

커링을 사용하면 함수를 부분적으로 구성할 수 있으며 재사용성을 높일 수 있다.

```javascript
const movies = [
  {
    "id": 1,
    "name": "Matrix"
  },
  {
    "id": 2,
    "name": "Star Wars"
  },
  {
    "id": 3,
    "name": "The wolf of Wall Street"
  }
]

const series = [
  {
    "id": 4,
    "name": "South Park"
  },
  {
    "id": 5,
    "name": "The Simpsons"
  },
  {
    "id": 6,
    "name": "The Big Bang Theory"
  }
]
```

위 코드에서 객체들에서 `id`를 추출해야 한다고 가정하면

1. `map()`을 이용한 반복(iterate)을 통해서  `id`를 추출하는 경우

   ```javascript
   movies.map((movie) => movie.id) // [ 1, 2, 3 ]
   series.map((serie) => serie.id) // [ 4, 5, 6 ]
   ```

2. 커링의 개념을 적용해 `get()`을 이용해서 `id`를 추출하는 경우

   ```javascript
   const get = property => object => object[property];
   const getId = get('id');
   
   movies.map(getId); // [ 1, 2, 3 ]
   series.map(getId); // [ 4, 5, 6 ]
   ```

1.의 경우 모두 다른 컬렉션에서 `map`을 통해 프로퍼티를 추출해야하지만 커링의 개념을 적용한 2.의 경우 객체에서 프로퍼티를 추출하는 함수인 `get()`을 통해 만들어낸 `getId`를 `map`의 인자로 사용함으로써 효율적으로 원하는 결과를 얻을 수 있다.

<br>

### Function Composition

함수 컴포지션(function composition)은 커링의 개념을 적용해 간단한 기능을 가진 함수를 여러개를 결합해서 복잡한 함수를 만드는 방법이다.

함수 컴포지션에 자동화(automate the function composition)를 적용해 고차 함수(high order function)를 만들 수 있다.

```javascript
const users = [
  { name: "Jeff", age: 14 },
    { name: "Jack", age: 18 }, 
    { name: "Milady", age: 22 },
]

const filter = (cb, arr) => arr.filter(cb);
const map = (cb, arr) => arr.map(cb);

// filter와 map을 이용해 function composition
map(u => u.name, filter(u => u.age >= 18, users)); // ["Jack", "Milady"]

// automate the function composition
const compose = (...functions) => args => functions.reduceRight((arg, fn) => fn(arg), args);
const filter = cb => arr => arr.filter(cb);
const map = cb => arr => arr.map(cb);

compose(
  map(u => u.name),
  filter(u => u.age >= 18)
)(users) // ["Jack", "Milady"]

```

- `compose()`는 다른 함수를 반환하는 고차 함수이며 다수의 함수를 인자로 가지며 `...` 연산자를 사용해 함수 배열로 변환한다.

<br>

### Pipe and Compose

#### Pipe

`pipe()`는 여러 함수를 연속적으로 실행하기 위한 패턴이며 인자로 받는 함수들의 순서대로(왼쪽에서 오른쪽) 호출된다.

```javascript
// 단항 구현(Unary implementation)
pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

pipe(add, multiply, double,divide)(5)
```

다항으로 구현할 경우의 코드와 과정은 아래와 같다.

```javascript
// 다항 구현(Multiple operator implementation)
pipe = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));

pipe(
    add,
    multiply
)(10, 10);
```

![pipe](https://i.imgur.com/s17rzwv.png)



#### Compose

`compose()`는 `pipe()`와 같은 기능을 가지며 인자로 받는 함수들의 오른쪽부터 왼쪽의 순서로 호출된다.

```javascript
compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);

// add부터 시작
compose(divide, double, multiply, add)(5)
```

<br>

<br>

------

**Reference**

- [Currying in JavaScript](https://www.codementor.io/@michelre/currying-in-javascript-g6212s8qv)
- [Use function composition in JavaScript](https://www.codementor.io/@michelre/use-function-composition-in-javascript-gkmxos5mj)
- [Javascript: Pipe and Compose functions](https://aparnajoshi.netlify.app/javascript-pipe-and-compose-functions)
- [함수형 프로그래밍: partial application과 curry](https://blog.rhostem.com/posts/2017-04-20-curry-and-partial-application)