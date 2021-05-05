# async/await

`async`와 `await`는 promise를 쉽게 사용할 수 있는 특별한 문법이다.

<br>

### Async functions

`async` 키워드는 `function`앞에 올 수 있으며 항상 promise를 반환한다.

promise가 아닌 값을 반환하는 경우에도 자동으로 그 값을 promise로 감싸서 반환한다.

``` javascript
async function func() {
	return 1;
}

func(); // Promise {<fulfilled>: 1}
func().then(alert) // 1
```

- `func()`를 호출하면 `1`의 값을 가진 이행상태의 `Promise`가 반환된다.

```javascript
async function func() {
  return Promise.resolve(1);
}

func().then(alert); // 1
```

- 명시적으로 `Promise`를 반환할 수도 있다.

<br>

### Await

`await`는 `async`함수 안에서만 동작하며 일반 함수에서는 `await`를 사용할 수 없다.

```javascript
// 1초후에 promise가 resolve되는 예시

async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // (*) promise가 이행될때까지 기다림

  alert(result); // "done!"
}

f();
```

- `(*)` 라인에서 함수 실행이 일시중지되고 promise가 처리(settled)되었을때 `result`에 promise의 결과값이 할당된다.

`await`는 말 그대로 promise가 처리가 끝날때까지 함수 실행을 중지하고 promise가 처리되면 그 결과와 함께 다시 함수 실행을 시작한다. 이러한 과정에서 자바스크립트 엔진은 다른 스크립트 실행, 이벤트 처리 등과 같은 다른 작업을 수행할 수 있기 때문에 CPU 리소스를 소모하지 않는다.

<br>

### Promise chaining to async-await syntax

```javascript
fetch('/article/promise-chaining/user.json')
  .then(response => response.json())
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  .then(response => response.json())
  .then(githubUser => new Promise(function(resolve, reject) {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => {
      img.remove();
      resolve(githubUser);
    }, 3000);
  }))
  // triggers after 3 seconds
  .then(githubUser => alert(`Finished showing ${githubUser.name}`));
```

위 코드의 `.then`을 `await`로 바꾸고 `await`를 쓰기 위해 함수에 `async`를 붙여주면 깔끔하고 가독성 좋게 바꿀수 있다.

```javascript
async function showAvatar() {

  // read our JSON
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();

  // read github user
  let githubResponse = await fetch(`https://api.github.com/users/${user.name}`);
  let githubUser = await githubResponse.json();

  // show the avatar
  let img = document.createElement('img');
  img.src = githubUser.avatar_url;
  img.className = "promise-avatar-example";
  document.body.append(img);

  // wait 3 seconds
  await new Promise((resolve, reject) => setTimeout(resolve, 3000));

  img.remove();

  return githubUser;
}

showAvatar();
```

<br>

### Error handling

일반적으로  promise가 resolve하면 `await promise`는 결과를 반환한다. 그러나 reject되는 경우에는 에러를 던지게된다.

```javascript
async function f() {
  await Promise.reject(new Error("Whoops!"));
}

async function f2() {
  throw new Error("Whoops!");
}

// f()와 f2()는 동일한 함수
```

`try..catch`구문을 사용해 에러를 잡을 수 있다. 에러가 발생할 경우 제어가 `catch`블록으로 넘어가게된다.

```javascript
async function f() {

  try {
    let response = await fetch('/no-user-here');
    let user = await response.json();
  } catch(err) {
    // catches errors both in fetch and response.json
    alert(err);
  }
}

f();
```

`async` 함수에서 `try..catch` 구문이 없을 경우 호출에 의해 생성된 promise는 reject된다.  이것을 처리하기 위해 `.catch`를 추가 할 수 있다.

```javascript
async function f() {
  let response = await fetch('http://no-such-url');
}

// f() becomes a rejected promise
f().catch(alert); // TypeError: failed to fetch // (*)
```

<br>

### Thumb Rules for async-await

1. `async` 함수는 `Promise`를 반환한다.

2. `async` 함수는 명시적(explicit)으로 `Promise`를 반환하지 않더라도 암시적(implicit)으로 `Promise`를 반환한다.

3. `await`는  `async` 함수안에서 `await statement`의 일부가 되는 코드 실행을 차단한다.

4. 하나의 `async` 함수 안에서 여러개의 `await` statements를 사용할 수 있다.

5. `async await`를 사용할때 `try catch`를 이용한 에러 핸들링을 하는 것이 좋다.

6. `await`는 항상 하나의 `Promise`가 이행될때 까지 기다린다.

7. `Promise`를 생성하면 비동기 기능의 실행이 시작된다.

<br>

<br>

------

**Reference**

- [Async/await](https://javascript.info/async-await)
- [JavaScript: Promises or async-await](https://betterprogramming.pub/should-i-use-promises-or-async-await-126ab5c98789)
