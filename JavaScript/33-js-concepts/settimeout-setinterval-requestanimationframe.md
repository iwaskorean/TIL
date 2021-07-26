#  setTimeout, setInterval and requestAnimationFrame

`setTimeout`과 `setInterval`은 측정 시간이 지난 후에 함수를 실행시키기 위한 메소드이며 자바스크립트가 아닌 브라우저 API(WebAPI)와 Node.js에서 지원한다.

<br>

### setTimeout

```javascript
let timerId = setTimeout(func, [delay], [args1], [args], ...)
```

`setTimeout` 메소드는 타이머가 완료된 후 실행되는 함수인 `func`와 타이머를 설정하는 `delay`, `func`에 전달되는 파라미터인 `args`를 인자로 가진다. 

`setTimeout` 메소드는 타이머가 완료되면 `func`를 1회 실행시킨다.

```javascript
function sayHi() {
  alert('Hi');
}

setTimeout(sayHi, 1000);
// 1000ms후에 alert "Hi"출력
```

```javascript
function sayHi(phrase, who) {
  alert( phrase + ', ' + who );
}

setTimeout(sayHi, 1000, "Hi", "Han"); // Hi, Han
```

`setTimeout`은 number타입의 timer 식별자를 리턴하는데, `clearTimeout`을 이용해 타이머가 완료된 후 함수가 실행되는 것을 취소 할 수있다.

```javascript
let timerId = setTimeout(() => {
	alert('Hi');
), 10000};

clearTimeout(timerId);
```

<br>

### setInterval

```javascript
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

`setInterval`은 `setTimeout`과 마찬가지로 타이머 완료 후 실행되는 함수인 `func`와 타이머를 설정하는 `delay`, `func`에 전달되는 파라미터인 `args`를 인자로 가진다. 

`setInterval`은 타이머 간격에 따라 주기적으로 `func`를 실행하며 `setTimeout`과 같이 `clearInterval`을 통해 주기적으로 함수가 실행되는것을 취소 할 수 있다.

```javascript
let timerId = setInterval(() => alert('tick'), 2000);

setTimeout(() => { clearInterval(timerId); alert('stop'); }, 5000);

// 2초, 4초에 'tick' alert 출력
// 5초에 'stop' alert 출력 후 setInterval 실행 중지
```

<br>

`setTimeout`과 `setTimeout`의 차이점은 아래와 같다.

- `setInterval`은 이전 함수 호출의 상태와 상관 없이 시간 간격마다 함수를 콜 스택에 전달하는 반면,  `setTimeout`은 함수가 실행되고 새로운 호출이 있을때만 다음 호출을 실행한다.

- 예를 들어, `setInterval`의 시간간격이 1000ms이고 함수를 실행하는 시간이 300ms일때, 호출 종료와 다음 호출까지의 시간 간격은 700ms가 된다.


`setInterval`대신  `setTimeout`을 재귀적으로 이용할 수 있다.

```javascript
function func() {
    console.log("It's been 2 second");
    setTimeout(repeatingFunc, 2000);
}

setTimeout(func, 2000);

// setTimeout을 재귀적으로 작성해 func를 2초마다 실행
```

- 브라우저 환경과 전체적인 코드흐름에 따라 발생하는 약간의 인터벌 지연을 방지할 수 있다..

- 비동기 작업의 경우 매우 비효율적인 긴 요청 대기열을 만드는 것을 방지할 수 있다.

<br>

### requestAnimationFrame

```javascript
function repeatOften() {
  // Do whatever
  requestAnimationFrame(repeatOften);
}
requestAnimationFrame(repeatOften);
```

`requestAnimationFrame`은 `setInterval`의 인터벌을 매우 작게 했을 때 요청이 지연되는 문제를 해결할 수 있는 메소드이다.

`setInterval` 비교해 `requestAnimationFrame`이 가지는 장점은 다음과 같다.

- 브라우저에서 최적화 할 수 있어 더 부드러운 애니메이션 효과를 적용할 수 있다.
- 비활성화된 탭에서는 애니메이션이 중지되어 CPU를 효율적으로 운용할 수 있다.
- 배터리를 절약할 수 있다.

- `cancelAnimationFrame`을 통해 함수의 실행을 취소 할 수 있다.

<br>

<br>


------

**Reference**

- [Scheduling: setTimeout and setInterval](https://javascript.info/settimeout-setinterval)
- [Using requestAnimationFrame](https://css-tricks.com/using-requestanimationframe/)
- [setTimeout VS setInterval](https://develoger.com/settimeout-vs-setinterval-cff85142555b)



