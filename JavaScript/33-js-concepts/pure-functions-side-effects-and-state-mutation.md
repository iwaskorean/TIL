# Pure Functions, Side Effects and State Mutation

함수형 프로그래밍에서의 함수란 인자(argument)를 입력으로 가지고 반환 값(return value)을 출력으로 생성하는 프로세스이다.

함수는 다음과 같은 3가지의 목적으로 사용될 수 있다.

- mapping :  주어진 입력을 기반으로 출력을 생성한다. 함수는 입력 값을 출력 값에 맵핑한다.

- procedures : 함수는 일련의 단계를 수행하기 위해 호출될 수 있다. 이 일련의 과정들을 프로시저라고 하며 이러한 스타일의 프로그래밍을 프로시저 프로그래밍이라고 한다.

- I/O : 화면, 저장 공간, 시스템 로그 또는 네트워크 등과 같은 시스템의 다른 부분과 통신할 수 있는 기능들이 있다.

<br>

### Mapping

함수는 입력 인자에 반환 값을 맵핑하는데 이것은 각 입력들의 집합에 출력이 있음을 의미한다.

순수 함수(pure function)는 이러한 맵핑에 관한 것이다.

```javascript
Math.max (2, 8, 5); // 8

// 2, 8, 5는 함수에 전달 된 값(argument)이다.
// Math.max()는 인자에 갯수에 제한없이 인자 값 중 가장 큰 값을 반환하는 함수이다.
```

수학에서도 함수가 있으며 수학의 함수도 자바스크립트의 함수와 비슷하게 동작한다.

```javascript
// f(x) = 2x, 
// f(2)는 항상 4라는 값을 가지므로 4를 f(2) 대신 쓸 수 있다.


// 이 함수를 자바스크립트의 함수로 변환하면
const double = x => x * 2;
double(5); // 10
```

double()은 순수함수이므로 자바스크립트 엔진은 double(5)를 10이라는 숫자로 대체 할 수 있다.
만약 double() 메소드가 디스크에 값을 저장하거나 콘솔에 로깅하는 등의 동작을 한다면 double(5)는 단순히 10이라는 숫자로 대체될 수 없다.

<br>

### Pure Functions

순수 함수란 같은 입력을 받으면 항상 같은 값의 출력을 반환하며 side-effect를 가지지 않는 함수이다.

순수 함수는 프로그램에서 가장 간단하면서 재사용 가능한 코드 구성 요소이다. 따라서 순수함수는 CS에서의 KISS(Keep It Simple, Stupid) 디자인 원칙을 지킬 수 있는 단순하고 간단한 방법이다.

순수 함수는 함수형 프로그래밍의 기반을 형성한다. 외부 상태와 완전히 독립적이며 이 특성은 많은 CPU와 분산 컴퓨팅 클러스터에서 병렬 처리를 위한 프로세싱 수행의 훌륭한 방법이 될 수 있다.

순수 함수는 매우 독립적이므로 리팩토링 및 재구성이 쉽고 프로그램을 더 유연하게 만들어준다.

<br>

### The Trouble with Shared State

모든 종류의 비동기 연산이나 동시 작업은 유사한 경쟁 상태(race condition)를 발생 시킬 수 있다. 

> 경쟁 상태(race condition) : 둘 이상의 입력이나 조작의 순서가 결과 값에 영향을 줄 수 있는 상태

API I/O, 이벤트 리스너, web workers API, Iframes, timeout 등의 자바스크립트 내부의 비동기 처리의 많은 소스들은 버그를 유발 할 수 있으며 순수 함수는 이러한 버그를 방지하는 도움이 될 수 있다.

<br>

### Given the Same Input, Always Return the Same Output

`double()` 함수의 예시처럼, 함수 호출을 결과 값으로 대체할 수 있으며 프로그램에서 `double(5)`는 항상 `10`과 같은 의미를 갖게 된다.

그러나 모든 함수에서 모두 적용되는것은 아니다. 일부 함수들은 인자값보다 다른 정보에 의존한다.

```javascript
Math.random(); // => 0.4011148700956255
Math.random(); // => 0.8533405303023756
Math.random(); // => 0.3550692005082965
```

`Math.random()` 함수 호출 시 어떤 인자도 전달되지 않았지만 모두 다른 출력을 만들어냈다. 따라서 `Math.random()`은 순수하지 않은 함수이다. 또한 `Math.random() `함수는 0과 1 사이의 새로운 난수를 생성해낸다. 0.3550692005082965라는 값을 `Math.random()`로 대체 할 수 없다는 것을 의미한다.

함수가 같은 입력 값에 항상 동일한 출력 값을 만들어낸다면 그 함수는 순수한 함수이다.

```javascript
const highpass = (cutoff, value) => value >= cutoff;

// The same input values
highpass(5, 5); // => true
highpass(5, 5); // => true

// Many input values
highpass(5, 123); // true
highpass(5, 6);   // true
highpass(5, 1);   // false
highpass(5, 3);   // false
```

순수 함수에서 동일한 입력 값들은 항상 같은 출력 값에 맵핑되며 다수의 입력 값들도 같은 출력 값에 맵핑 될 수 있다.

<br>

### Pure Functions Produce No Side Effects

순수 함수는 side effects를 만들어내지 않으며 이것은 어떠한 외부 상태도 바꿀 수 없다는 것을 의미한다.

<br>

### Immutability

자바스크립트에서 오브젝트는 참조타입이다. 함수가 오브젝트나 배열의 매개변수의 프로퍼티를 변경하면 함수 밖에서의 접근이 가능한 상태로 변화한다. 순수 함수는 외부 상태를 변화시켜선 안된다.

```javascript
// 순수하지않은 addToCart 함수는 존재하는 cart를 변화시킨다.
const addToCart = (cart, item, quantity) => {
  cart.items.push({
    item,
    quantity
  });
  return cart;
};


test('addToCart()', assert => {
  const msg = 'addToCart() should add a new item to the cart.';
  const originalCart =     {
    items: []
  };
  const cart = addToCart(
    originalCart,
    {
      name: "Digital SLR Camera",
      price: '1495'
    },
    1
  );

  const expected = 1; // num items in cart
  const actual = cart.items.length;

  assert.equal(actual, expected, msg);

  assert.deepEqual(originalCart, cart, 'mutates original cart.');
  assert.end();
});
```

순수하지 않은(impure) `addToCart()`함수를 순수하게 변경하면

```javascript
// 순수한 addToCart 함수는 새로운 cart를 리턴한다.
// 기존의 객체를 변화시키지 않는다.
const addToCart = (cart, item, quantity) => {
  const newCart = lodash.cloneDeep(cart);

  newCart.items.push({
    item,
    quantity
  });
  return newCart;

};


test('addToCart()', assert => {
  const msg = 'addToCart() should add a new item to the cart.';
  const originalCart = {
    items: []
  };

  // deep-freeze on npm
  // throws an error if original is mutated
  deepFreeze(originalCart);

  const cart = addToCart(
    originalCart,
    {
      name: "Digital SLR Camera",
      price: '1495'
    },
    1
  );


  const expected = 1; // num items in cart
  const actual = cart.items.length;

  assert.equal(actual, expected, msg);
```

<br>

<br>


------

**Reference**

- [Master the JavaScript Interview: What is a Pure Function?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

