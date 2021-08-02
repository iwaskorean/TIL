# Object.create and Object assign

## Object.create()

`Object.create()`는 ES5에서 도입되었으며, 지정된 프로토타입으로 새로운 객체를 만드는 메소드이다.

새로 생성된 객체는 모든 프로토타입 객체 프로퍼티를 상속한다.

```javascript
// const newObject = Object.create(prototype)

const animal = {}
const dog = Object.create(animal)
```

두 번째 파라미터를 사용해 프로토타입에 없는 새로운 프로퍼티를 객체에 추가 할 수 있다.

```javascript
// const newObject = Object.create(prototype, newProperties)

const animal = {}
const dog = Object.create(animal, {
  breed: {
    value: 'Siberian Husky'
  }
});

console.log(dog.breed) //'Siberian Husky'
```

<br>

아래 코드는 `Object.create()` 메소드를 사용해 객체를 생성하는 예제이다.

```javascript
var dog = { 
    eat: function () { 
        console.log (this.eatFood) 
    } 
}; 

var maddie = Object.create(dog); 

console.log (dog.isPrototypeOf(maddie)); // true 

maddie.eatFood = 'NyamNaym'; 
maddie.eat(); // NyamNaym
```

1. `eat` 메소드를 가지고 있는 `dog` 객체 생성한다.
2. `Object.create(dog)`을 사용해 `maddie`를 초기화하고 `dog`의 프로토타입으로 설정된 새로운 완전히 새로운 객체를 만든다.
3. 자바스크립트는 프로토타입 체인을 통해 `dog`에서 `eat` 메소드를 찾아내고 `this`를 `maddie`로 설정한다. 
4. `Object.create()`는 프로토타입이 `dog`로 설정된 `maddie`를 새롭게 만들었으므로 `maddie` 객체는 `dog`의 메소드에 접근 가능하다.

<br>

아래 코드는 new 연산자를 사용해 객체를 생성하는 예제이다.

```javascript
var Dog = function(){
    this.eatFood = 'NyamNyam';
    this.eat = function(){
        console.log(this.eatFood)
    }
};J

var maddie = new(Dog);

console.log(maddie instanceof Dog); // True

maddie.eat(); // NyamNyam
```

1. `maddie`라는 새로운 객체를 `new` 연산자로 생성하며 `maddie`는 생성자 함수의 프로토타입을 상속한다.
2. 생성된 객체로 `this` 설정되고 constructor가 실행된다.
3. 생성된 객체가 반환된다. (constructor가 객체를 반환하지 않은 경우)

<br>

### The difference with Object.create() and new operator

```javascript
function Dog(){
    this.pupper = 'Pupper';
};

Dog.prototype.pupperino = 'Pups.';
var maddie = new Dog();
var buddy = Object.create(Dog.prototype);

// Object.create()를 사용한 객체 생성
console.log(buddy.pupper); // ***, undefined
console.log(buddy.pupperino); // Pups.

// new 연산자를 사용한 객체 생성
console.log(maddie.pupper); // Pupper
console.log(maddie.pupperino); // Pups.
```

`new` 연산자로 생성된 `maddie`는 생성자 코드를 실행하지만 `Object.create()`를 사용해 생성된 `buddy`는 생성자 코드를 실행하지 않는다. 

따라서 `buddy`는 생성자에서 `this.pupper`에 접근 할 수 없어 `buddy.pupper`는 `undefined`를 반환한다(`***`).

<br>

<br>

## Object.assign()

`Objcet.assgin()`은 ES6에서 도입되었으며, 하나 이상의 객체의 열거 가능한(enumerable) 프로퍼티를 다른 객체로 복사하는 메소드이다.

```javascript
const copied = Object.assign({}, original);
```

값 복제 및 객체 참조가 복사되는 얕은 복사(shallow copy)이므로 원본 객체의 프로퍼티를 수정하면 복사된 객체의 프로퍼티 또한 변경된다.

```javascript
const original = {
  name: 'toyota',
  car: {
    color: 'black'
  }
}

const copied = Object.assign({}, original);

copied.name // toyota
copied.color // black

// 원본 객체 프로퍼티 수정
original.name = 'bmw'
original.car.color = 'white'

copied.name // bmw
copied.car.color // white
// 얕은 복사이므로 copied의 프로퍼티 또한 변경됨
```

<br>

<br>

------

**Reference**

- [The Object create() method](https://flaviocopes.com/javascript-object-create/)
- [The Object assign() method](https://flaviocopes.com/javascript-object-assign/)
- [Understanding the difference between Object.create() and the new operator.](https://medium.com/@jonathanvox01/understanding-the-difference-between-object-create-and-the-new-operator-b2a2f4749358)