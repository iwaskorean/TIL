# Inheritance, Polymorphism and Code Reuse

자바스크립트는 객체지향 언어이며 캡슐화, 추상화, 다형성 등 OOP의 개념들을 구현할 수 있다. 또한 prototype 상속이 가능하다.

<br>

### What is Inheritance

상속은 class-based 객체지향 언어에서 한 객체가 다른 객체의 모든 프로퍼티와 동작을 획득하는 중요한 메커니즘이다. 

ES6에서 class 키워드가 추가되었지만 자바스크립트는 class-based 언어가 아니기때문에 prototype 체인에서 동작한다.

<br>

### Classical Inheritance vs  Prototypal Inheritance

![img1](https://miro.medium.com/max/1050/1*UZKbYsRuM1OVW3__aZdPdw.png)

#### Classical Inheritance(non-javacript)

- `Vehicle`은 부모클래스이며 `v1`, `v2`는 `Vehicle`의 인스턴스이다.
- `Car`는 `Vehicle`의 자식클래스이며 `c1`, `c2`는 `Car`의 인스턴스이다.
- 클래스 상속은 클래스를 확장할때(extend) 부모클래스에서 자식클래스로 복사본을 만든다. 따라서 부모와 자식 클래스는 분리된 독립체이다.

#### Prototypal Inheritance

- `v1`과 `v2`는 `new` 키워드를 통해 생성되었기 때문에 `Vehicle.prototype`와 연결되어 있다.

- `c1`, `c2`는 `Car.prototype`과 연결되어 있으며 `Car.prototype`은 `Vehicle.prototype`과 연결되어있다.

- 자바스크립트에서는 객체를 생성할때 프로퍼티나 동작들을 복사하는 것이 아닌 링크를 만든다.  이러한 링크들을 prototype 체인이라고하며 class를 확장할때도 마찬가지로 적용된다.

- 위와 같은 패턴을 Behavior Delegation Pattern이라고 부른다.

  <br>

### Example

![img2](https://miro.medium.com/max/1050/1*OEMd8RbQYOc3aonG4MY0EQ.png)

```javascript
// Vehicle - 슈퍼클래스
function Vehicle(name) {
  this.name = name;
}

// 슈퍼클래스에서 정의한 start()
Vehicle.prototype.start = function() {
  return "engine of "+this.name + " starting...";
};

// Car - 서브클래스
function Car(name) {
  Vehicle.call(this,name); // call super constructor.
}

Car.prototype = Object.create(Vehicle.prototype);

// 서브클래스 Car에서 정의한 run()
Car.prototype.run = function() {
  console.log("Hello "+ this.start());
};

// 서브클래스 Car의 인스턴스
var c1 = new Car("Fiesta");
var c2 = new Car("Baleno");

// accessing the subclass method which internally access superclass method
// 내부적으로
c1.run();   // "Hello engine of Fiesta starting..."
c2.run();   // "Hello engine of Baleno starting..."
```

- `c1`에는 `run()`과 `start()` 메소드가 없지만 위쪽으로 이동할 수 있는 링크 즉, prototype 체인에 의해서 `run()`과 `start()`에 액세스 할 수 있다.
- `new` 키워드가 아닌 `Object.create()`를 사용해 객체간의 연결을 생성할 수 있다. = Object delegation pattern

<br>

### Polymorphism

> Poly = many, Morphism = form

객체지향에서의 다형성이란 상속된 메소드를 재정의(overriding)하여 다른 객체에서 동일한 메소드를 쉽게 호출할 수 있도록 하는 것이다. 재정의된 메소드는 세부적인 프로퍼티들은 다르지만 같은 목적을 가지며 다른 데이터 유형을 조작할 수 있다.

다형성은 모듈화 및 코드 재사용성을 높이는데 유용하며 타입 시스템을 유연하게 한다.

```javascript
var shape = function(){};

shape.prototype.draw = function(){
	return "i am generic shape";
}

//circle
var circle = function(){}
circle.prototype = Object.create(shape.prototype);
//overriding
circle.prototype.draw= function(){
	return "i am a circle";
}

//triangle
var triangle = function (){}
triangle.prototype = Object.create(shape.prototype);
//overriding
triangle.prototype.draw= function(size){
	return "this is triangle";
}

//printing shapes
var shapes = [new shape(), new circle(), new triangle(23)];
shapes.forEach (function (shapeList){
	console.log(shapeList.draw());
});

// i am generic shape
// i am a circle
// this is triangle
```

<br>

------

**Reference**

- [JavaScript — Inheritance, delegation patterns and Object linking](https://codeburst.io/javascript-inheritance-25fe61ab9f85)
- [Object Oriented JavaScript: Polymorphism with examples](https://blog.knoldus.com/object-oriented-javascript-polymorphism-with-examples/)
