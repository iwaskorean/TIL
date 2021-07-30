# this, call, apply and bind

### this

함수가 만들어질때, 함수가 동작하는 곳의 객체와 연결하는 `this`라는 키워드가 만들어진다.

`this` 키워드의 값은 함수 자체와 상관없으며 함수가 어떻게 호출되는지가 값을 결정한다.

<br>

### Default `this` context

```javascript
const myFunc = function () {
  console.log(this);
};

myFunc();

/*
    Window {0: global, window: Window, self: Window, document: document, name: "", location: Location, …}
*/
```

기본적으로 `this`가 참조하는 값은 글로벌 스코프를 참조하는 window Object 이다. 스크립트가 엄격한 모드(use strict)로 작동하고 있다면 `this`는 `undefined` 값을 갖는다.

<br>

### Implicit Binding

```javascript
const myObj = {
  myMethod: function () {
    console.log(this);
  }
};
```

`this` 키워드의 값은 함수 자체가 함수를 호출하는 방법에 따라 `this`의 값을 결정되므로 위 코드의 `this`의 값은 알 수 없다.

```javascript
const myMethod = function () {
  console.log(this);
};

const myObj = {
  myMethod: myMethod
};

myObj.myMethod() // this === myObject
myMethod() // this === window
```

`myObj`는 `myMethod`를 프로퍼티로 가지며 이 프로퍼티는 `myMethod` 함수를 가르킨다.  `myMethod` 함수가 글로벌 스코프에서 호출될 경우 `this`는 window object를 참조한다. `myMethod` 함수가 `myObj`의 메소드로 호출될 경우 `this`는 `myObj`를 가르킨다.

이것을 `this`의 암시적 바인딩이라고 한다.

<br>

### Explicit Binding

`call()` 또는 `apply()` 메소드를 통해 명시적으로 함수에 컨텍스트를 바인딩할 수 있다.

- `call()` :  함수가 호출 될때 첫번째 파라미터 객체를  `this` 값으로 설정한다. 나머지 파라미터들은 실제 함수에 대한 인자이다.
- `apply()` : `call()`과 유사하게 첫번째 파라미터 객체를 `this` 값으로 설정한다. 다른점은 두번째 파라미터에서 실제 함수의 인자를 배열로 받는다.

```javascript
const myMethod = function () {
  console.log(this);
};

const myObj = {
  myMethod: myMethod
};

myMethod() // this === window
myMethod.call(myObj, args1, args2, ...) // this === myObject
myMethod.apply(myObj, [array of args]) // this === myObject
```

명시적 바인딩과 암시적 바인딩이 같이 있는 경우 명시적 바인딩이 더 높은 우선순위를 갖는다.

```javascript
const myMethod = function () { 
  console.log(this.a);
};

const obj1 = {
  a: 1,
  myMethod: myMethod
};

const obj2 = {
  a: 2,
  myMethod: myMethod
};

obj1.myMethod(); // 1
obj2.myMethod(); // 2

obj1.myMethod.call( obj2 ); // 2
obj2.myMethod.call( obj1 ); // 1
```

<br>

### Hard Binding

하드 바인딩이란 `bind()` 메소드를 통한 바인딩이다, `bind()` 메소드는 우리가 특정한 `this` 컨텍스트를 가진 원래의 함수를  불러오기위해 하드 코딩된 새로운 함수를 리턴한다.

- `bind()` : `callO()`, `apply()`와 다르게 함수를 실행하지 않고 나중에 실행될 컨텍스트를 가진 bound 함수를 리턴한다.

```javascript
myMethod = myMethod.bind(myObject);

myMethod(); // this === myObject
```

하드 바인딩은 명시적 바인딩 보다 높은 우선순위를 갖는다.

```javascript
const myMethod = function () { 
  console.log(this.a);
};

var obj1 = {
  a: 1
};

var obj2 = {
  a: 2
};

myMethod = myMethod.bind(obj1); // 1
myMethod.call( obj2 ); // 1
```

<br>

### New Binding

new 바인딩이란 `new` 키워드를 사용한 바인딩이다. `new`를 통해 함수가 호출되었을때는 암시적/명시적/하드 바인딩과 상관없이 새로운 인스턴스인 새로운 컨텍스트를 만든다.

```javascript
function foo(something) {
  this.a = something;
}

const obj1 = {};

const bar = foo.bind( obj1 ); // hard binding
bar( 2 );

console.log( obj1.a ); // 2

const baz = new bar( 3 ); // new 키워드를 통한 new binidng

console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```

<br>

### API Calls

Ajax, event handler 등과 같은 라이브러리나 헬퍼 오브젝트를 사용해 콜백 함수를 호출할때 주의해야한다.

```javascript
myObject = {
  myMethod: function () {
    helperObject.doSomethingCool('superCool',
      this.onSomethingCoolDone);
    },

    onSomethingCoolDone: function () {
      /// Only god knows what is "this" here
    }
};
```

`this.onSomethingCoolDone`를 콜백으로 전달했기 때문에 스코프가 메소드를 참조했다고 볼 수도 있지만 `this`의 참조를 알 수 없다.

위 코드를 올바르게 수정하는 방법은 아래와 같다.

```javascript
// #1 
// 라이브러리는 다른 파라미터들을 제공하므로 가져오길 원하는 스코프를 전달 할 수 있다.
myObject = {
  myMethod: function () {
    helperObject.doSomethingCool('superCool', this.onSomethingCoolDone, this);
  },

  onSomethingCoolDone: function () {
    /// Now everybody know that "this" === myObject
  }
};

// #2
// 원하는 스코프를 하드 바인딩을 통해 가져올 수 있다.
myObject = {
  myMethod: function () {
    helperObject.doSomethingCool('superCool', this.onSomethingCoolDone.bind(this));
  },

  onSomethingCoolDone: function () {
    /// Now everybody know that "this" === myObject
  }
};

// #3
// 클로저를 만들고 this를 me에 캐시할 수 있다. (권장하지 않음)
myObject = {
  myMethod: function () {
    var me = this;

    helperObject.doSomethingCool('superCool', function () {
      /// Only god knows what is "this" here, but we have access to "me"
  });
  }
};
```

<br>

<br>

------

**Reference**

- [Understanding "This" in JavaScript](https://www.codementor.io/@dariogarciamoya/understanding--this--in-javascript-du1084lyn?icn=post-8i1jca6jp&ici=post-du1084lyn)
- [How-to: call() , apply() and bind() in JavaScript](https://www.codementor.io/@niladrisekhardutta/how-to-call-apply-and-bind-in-javascript-8i1jca6jp)