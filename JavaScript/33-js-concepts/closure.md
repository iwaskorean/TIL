# Closure

###### *이전 정리 포스트 : [Function, Block, and Lexical Scope - Closer](https://github.com/SewookHan/TIL/blob/main/JavaScript/33-js-concepts/function-block-lexical-scope.md#closer)*

> 클로저는 독립적인 (자유) 변수를 참조하는 함수이다. 클로저에 정의된 함수는 생성될때의 환경을 '기억'한다. [MDN]
>

클로저가 나타나는 가장 기본적인 환경은 함수안에 함수가 다시 선언되어 호출되었을 때이다.

```javascript
function outer () { 
	var count = 0; 
	return { // #1
		increase: function () { 
			return ++count; 
		}, 
        decrease: function () { 
			return --count; 
		} 
	}; 
} 

var counter = outer(); // #2
counter.increase(); // === 1
counter.increase(); // === 2
counter.decrease(); // === 1

var counter2 = outer(); 
counter.increase(); // === 2 #3
```

`#2`에서 `outer()` 함수를 호출할 때 `#1`에서 선언한 `object`가 리턴이 되어 `counter`에 들어가게된다. 이 경우에 클로저가 생성되어 `increase`와 `decrease` 두개의 함수가 동일 스코프에서 같은 작업을 할 수 있다.

`outer()` 함수를 한번 더 호출 할 경우 `counter`와 `counter2`는 서로 다른 스코프를 생성해 `outer()` 내부의 `count`변수를 따로따로 저장하게 된다.(#3)

<br>

### Examples

#### functionFactory

```javascript
var functionFactory = function(num1) {
    return function(num2) {
        return num1 * num2;
    }
}

// #1
var mult5 = functionFactory(5);
var mult10 = functionFactory(10);

// #2
> mult5(3)
15
> mult5(5)
25
> mult10(3)
30
> mult10(5)
50
```

`#1` : single number를 `functionFactory()`에 전달해 `functionFactory()`에  `num1`의 값을 기억하는 클로저를 리턴한다.

`#2` : 결과 함수는 `num1`의 값에 전달된 `num2`를 곱한다.

<br>

#### sum()

```javascript
function sum(base) { 
    var inClosure = base; // #1 
    return function (adder) { // #2 
	    return inClosure + adder; 
    }; 
}; // #3 

var fiveAdder = sum(5); // #4: inClosure를 5로 설정하고 #2의 함수 리턴 

fiveAdder(3); // === inClosure(5) + adder(3) === 8 

var threeAdder = sum(3); // #5: inClosure를 3으로 설정하고 #2의 함수 리턴
```

![img](https://t1.daumcdn.net/cfile/tistory/122FC7455108833B38)



`#4) var fiveAdder = sum(5)` : `inCloser`를 5로 설정하고 `inCloser`를 참조하는 내부 함수를 리턴하여 `fiveAdder`에 대입한다. (A)

`#5) var threeAdder = sum(3)` :  `inCloser`를 3으로 설정하고 또 다른 스코프 체인이 생성되어 `threeAdder`에서 사용하게 된다. (B)

이렇게 클로저를 통해 각 함수들은 자기만의 고유한 값을 가지고 스코프 체인을 유지하면서 체인 내부의 변수의 값을 유지할 수 있다.

<br>

<br>

------

**Reference**

- [CLOSURE 쉽게 이해하기 / 실용 예제 소스](https://unikys.tistory.com/309)
- [JavaScript Closures by Example](https://howchoo.com/g/mge2mji2mtq/javascript-closures-by-example)