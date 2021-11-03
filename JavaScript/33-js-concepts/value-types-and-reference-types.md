# Value Types and Reference Types

자바스크립트에서 원시 타입(primitive types)인 `Boolean`, `null`, `undefined`,`String`, `Number` 타입은 값에 의한 전달(passed by value)이 일어나며 원시 타입이 아닌(non-primitive types) Object 타입은 참조에 의한 전달(passed by reference)이 일어난다.

<br>

### Primitives

원시 타입의 값이 변수에 할당되면 그 변수는 원시 값을 포함하는 것으로 생각할 수 있다.

```javascript
var x = 10;
var y = 'abc';
var z = null;
```

`x`는 10이란 값을 가지고 있고, `y`는 'abc'를 가지고 있다. 이를 메모리 상의 이미지로 나타내면 아래와 같다.

![image](https://miro.medium.com/max/1050/1*PdKLlT7zUrmDBZUOBsZh7w.png)

<br>

이 변수들을 다른 변수에 `=`을 이용해 할당할 때, 새로운 변수에 값을 복사한다. 새로운 변수들은 값에 의해 복사되어진다.

```javascript
var x = 10;
var y = 'abc';

var a = x;
var b = y;

console.log(x, y, a, b); // -> 10, 'abc', 10, 'abc'
```

![img2](https://miro.medium.com/max/1050/1*MZ3AcwELYZ2ONYFg3LiTXQ.png)

한 변수를 바꾸더라도 다른 변수는 변하지 않으며 각 변수들끼리 아무런 관계가 없다.

<br>

### Objects

원시 타입이 아닌 값이 할당된 변수들은 그 값들을 가르키는 참조(reference)를 갖게 된다. 변수는 실제로 값을 포함하고 있지 않으며, 참조는 메모리상의 객체의 위치를 가르키고 있다.

우리가 `arr = []` 이 코드를 작성했을때, 우리는 메모리 내부에 배열을 만든것이다. 변수 `arr`의 값은 배열의 위치한 주소를 갖는다.

```javascript
var arr = [];	// #1
arr.push(1);	// #2	
```

![img3](https://miro.medium.com/max/1050/1*h1aXuPwCyhu6GKwgeFMLDw.png)

![img4](https://miro.medium.com/max/1050/1*HaemMnuU05EW1b3BZPubIg.png)

`arr`이 가지고 있는 변수의 값(주소)은 정적(static), 변수의 값이 바뀌는 것이 아니라 메모리 속 배열의 값이 바뀌는 것이다.

<br>

### Assigning by Reference

Object와 같은 참조 타입의 값이  `=`를 이용해 다른 변수로 값이 복사 되어질때, 그 값의 주소가 실제로 복사된다.  즉, 값이 아닌 참조로 복사된다.

```javascript
var reference = [1];
var refCopy = reference;
```

![img](https://miro.medium.com/max/1050/1*d2W3ulHbHRGrFQ-c1SG5gA.png)

각각의 변수는 같은 배열을 가르키는 참조를 갖는다.

<br>

### Reassigning a Reference

참조 값을 재할당 하는 것은 오래된  참조(old Reference)를 대체한다.

```javascript
var obj = { first: 'reference' };
```

![img](https://miro.medium.com/max/1050/1*PWGp9d2zZ_QGg18HXBSq9Q.png)

이 메모리 상태에서 참조 값을 재할당 했을때의 메모리는

```javascript
var obj = { first: 'reference' };
obj = { second: 'ref2' }
```

![img](https://miro.medium.com/max/1050/1*1h73Wn9IyaiXbhxhmJZmYA.png)

첫번째 객체가 메모리상에 나타나지만, `Objects`안에 저장되었던 주소의 변경된다.

주소 값  `#234`와 같이 객체를 가르키는 참조가 남아있지 않을때 자바스크립트 엔진은 가비지 컬렉션(Garbage Collection)을 동작 시킬 수 있다.

> 가비지 컬렉션 : 프로그래머가 객체를 가르키는 모든 참조를 되찾을수 없고(has lost all references) 객체를 더이상 사용할 수  없게 되면 자바스크립트 엔진이 메모리로에서 사용되지 않는 객체를 안전하게 삭제하는것을 의미

위 메모리에서 `{first:'reference'}`가 더 이상 접근 불가능하므로 가비지 컬렉션의 대상이 될 수 있다.

<br>


### == and ===

`==`와 `===` 같은 동등 연산자(equality operators)는 참조 타입의 변수들의 참조를 비교할 때 사용된다.

변수들이 같은 아이템에 대한 참조를 갖고 있다면, 비교의 결과는 `true`일 것이다.

```javascript
// #1
var arrRef = [’Hi!’];
var arrRef2 = arrRef;
console.log(arrRef === arrRef2); // -> true

// #2
var arr1 = ['Hi!'];
var arr2 = ['Hi!'];
console.log(arr1 === arr2); // -> false

// #3. 문자열로 바꾸어 문자열을 비교, 원시 타입을 비교할때는 값이 같은지 확인함
var arr1str = JSON.stringify(arr1);
var arr2str = JSON.stringify(arr2);
console.log(arr1str === arr2str); // true
```

<br>

### Passing parameters though Functions

원시 타입의 값들을 함수로 전달할때 함수는 `=`를 사용해 할당한것 처럼 값을 파라미터에 복사한다.

```javascript
var hundred = 100;
var two = 2;
function multiply(x, y) {
    // PAUSE
    return x * y;
}
var twoHundred = multiply(hundred, two);
```

![img](https://miro.medium.com/max/1050/1*3AYcflNTwTTGTgCul0DgBw.png)

<br>

### **Pure Functions**

순수함수는 원시 값들만을 파라미터로 이용하고 주변 스코프에서 어떠한 변수들을 사용하지않는 함수이다. 순수함수에서 모든 변수들은 함수에서 리턴되자마자 가비지컬렉션 처리된다.

`Array.map`, `Array.filter`를 포함한 많은 배열 함수들은 위와 같은 이유로 순수 함수로 작성되어있다. 배열 참조를 받아서 내부적으로 배열을 복사하고 원본 대신 복사된 배열로 작동한다.

 따라서 순수함수는 원본은 건드리지않고 바깥 스코프에서 영향을 미치지않으며 우리는 새로운 배열의 참조를 리턴받게 된다.

```javascript
// 순수하지 않은 함수(impure function)
function changeAgeImpure(person) {
    person.age = 25;
    return person;
}
var alex = {
    name: 'Alex',
    age: 30
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // -> { name: 'Alex', age: 25 }
console.log(changedAlex); // -> { name: 'Alex', age: 25 }

// alex와 changedAlex는 같은 참조를 가지게 된다.
// person 변수를 리턴하고 새로운 변수에 그 참조를 저장하는것은 불필요하다
```

```javascript
// 순수 함수(pure function)
function changeAgePure(person) {
    var newPersonObj = JSON.parse(JSON.stringify(person));
    newPersonObj.age = 25;
    return newPersonObj;
}
var alex = {
    name: 'Alex',
    age: 30
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }

// 순수함수이므로 새로운 객체에서 age 프로퍼티를 변경할때 원본엔 아무런 영향을 끼치지않는다.
// 바깥 스코프에 아무런 영향을 끼치지않으며 심지어 인자로 전달받은 객체도 마찬가지이다.
```

<br>

### Test

```javascript
function changeAgeAndReference(person) {
    person.age = 25;
    person = {
        name: 'John',
        age: 50
    };
    
    return person;
}

var personObj1 = {
    name: 'Alex',
    age: 30
};

var personObj2 = changeAgeAndReference(personObj1);

console.log(personObj1); // -> ?
console.log(personObj2); // -> ?
```

```javascript
console.log(personObj1); // -> { name: 'Alex', age: 25 }
console.log(personObj2); // -> { name: 'John', age: 50 }
```

함수는 처음에 원본 객체로부터 전달받은 `age`프로퍼티를 바꾼다. 그리고 나서 변수에 새로운 객체를 다시 할당(reassigns). 그리고 그 객체를 리턴한다.

함수의 파라미터로 할당되는 것들은 `=`로 할당되는것과 같다.  `person`은 `personObj1`을 가르키는 참조를 가지고 있기 처음에 전달받은 `personObj1`에 직접 액션을 일으킨다.

새로운 객체에 `person`을 재할당한 후에는 원본 객체(`personObj1`)에 영향을 끼치지 않으며 `person`은 새로운 참조를 갖게 된다.

<br>

<br>


------

**Reference**

- [Explaining Value vs. Reference in Javascript](https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0)