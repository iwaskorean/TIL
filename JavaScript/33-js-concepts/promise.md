# Promise

> The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

<br>

### Introduction

![promise](https://res.cloudinary.com/practicaldev/image/fetch/s--SVoAXeRc--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/upv1hrcs93qsdmkehqs4.png)


콜백은 어떤 일이 발생하거나 완료되었을때 동작되는 함수이다. 만약 비동기 요청을 해야하는 경우 성공하거나 또는 실패할 수 있다. 이와같은 두 가능성은 `resolves` 또는 `rejects` 파라미터로 나타낼 수 있다.

Promise를 사용하면 `then`으로 체이닝 구조를 만들고 에러를 위해 `catch`를 사용해 직관적인 방식으로 콜백을 쉽게 연결 할 수 있으며 콜백 지옥(callback hell)을 피할 수 있다.

Promise는 아래와 같은 3가지의 상태를 갖는다.

- 대기(pending) : 초기상태이며 이행(fulfilled)되거나 거부(rejected)되지 않는다.

- 이행(fulfilled) : 작업이 성공적으로 완료됨을 의미한다.

- 거절(rejected) : 작업이 실패했음을 의미한다.

<br>
<br>

### Using `new Promise`

```javascript
let foo = new Promise((resolve, reject) => {resolve('foo')})
foo.then(value => console.log(value) 
// foo

// resolve, rejected를 Promise 메소드로 사용할 수 있다.
let foo = Promise.resolve('foo')
foo.then(value => console.log(value))
// foo
```
<br>
<br>

## Examples

### A Simple Promise

```javascript
const promise = new Promise(function (resolve, reject) {
  const a = 'apples';
  const b = 'apples';
  if (a === b) {
    resolve();
  } else {
    reject();
  }
});

promise
  .then(() => {
    console.log('success');
  })
  .catch(() => {
    console.log('error');
  });

// success
```

### Promise + setTimeout

```javascript
let promise1 = new Promise((resolve, reject) => {
  setTimeout(function() {
    resolve('foo');
  }, 2000)
}) // promise가 선언되면 시간이 흐르기 시작한다.(time starts ticking)

promise1.then(val => console.log(val)) 
console.log("I promise I'll be first!")

// I promise I'll be first!
// foo (2초 경과후 출력)

//promise1은 promise 객체이다.
promise1 
// Promise {<fulfilled>: "foo"}
```

### Continuous chaining

promise 체인에서 인자를 전달하기 위해서는 반드시 이전 `then` 반환하는 값이 있어야한다. 리턴하는 값이 없을 경우 다음 `then`은 인자를 가지지 않는다.

```javascript
let hello1 = new Promise(resolve => resolve("hello1"))

hello1.then(val1 => {
  console.log(val1);
  return "hello2"
}).then(val2 => {
  console.log(val2);
  return "hello3"
}).then(val3 => {
  console.log(val3)
})
// hello1 // hello2 // hello3

let hello2 = new Promise(resolve => resolve("hello1"))

hello2.then(val1 => {
  console.log(val1);
  return "hello2"
}).then(val2 => {
  console.log(val2); // 반환하는 값이 없음 
}).then(val3 => { 
  console.log(val3); // val3 is undefined
})

// hello // hello2 // undefined
```

### API Call

```javascript
let fetchTodo = fetch('https://jsonplaceholder.typicode.com/todos/1')

fetchTodo // Promise 반환, Promise {<pending>}

fetchTodo
  .then(response => response.json())
  .then(json => console.log(json))
```

### Catching Error and Rejection Handling

`then`을 사용할때마다 `catch`로 에러 처리를 해주어야하며 두번째 인자 `reject`를 이용해서 거절 상태 처리를 할 수 있다.

```javascript
foo = new Promise((resolve, reject) => {reject('error foo')})

// Bad, then만 사용했을때
foo.then(val => console.log(val))
// Promise {<rejected>: "error foo"}
// (error) Uncaught (in promise) error foo

// Good, catch로 에러 처리를 해주었을때
foo.then(val => console.log(val)).catch(err => console.log(err)) 
// error foo
```

### Promise Method finally()

`finally()` 메소드를 사용하면 에러 발생 여부에 관계없이 작업을 수행할 수 있다. `then()` 또는 `done()`의 호출 후 마무리로 많이 사용된다.

```javascript
spinnerShow(); // 로딩 중 스피너 실행 함수 호출

fetchData()
  .then((data) => renderPage(data))
  .catch(error)
  .finally(spinnerHide); // 페이지 로드 후 로드 성공 여부에 관계없이 스피너 중지
```

<br>

<br>

------

**Reference**

- [Promises in JavaScript](https://dev.to/peterklingelhofer/promises-in-javascript-3h5k)
- [Javascript Promise 101](https://dev.to/iggredible/javascript-promise-101-3idl)
- [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
