#  Factories and Classes

## Class

자바스크립트에서의 클래스는 프로토타입을 기반으로 만들어진 특수한 함수이다. 따라서 함수를 정의하듯 클래스 선언과 표현식으로 정의 할 수 있다. 

<br>

### Class Declarations

`class` 키워드를 사용해 클래스 선언을 통한 정의가 가능하다.

`function` 키워드를 통한 함수 선언은 호이스팅이 일어나지만 `class` 키워드를 통한 클래스 선언은 클래스를 사용하기 전에 선언 해야한다.

```javascript
const c = new Rect(); // ReferenceError

class Rect {
    ...
}
```

클래스 표현식을 통한 클래스 정의는 클래스 이름을 가질 수도 있고, 그렇지 않을 수도 있다.

클래스 표현식으로 정의된 클래스 이름은 함수 표현식과 마찬가지로 내부 범위에서만 유효하며 사용된 클래스 이름은 외부 범위에서 접근 불가하다.

```javascript
// unnamed
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle"

// named
let Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle2"
```

<br>

### Strict Mode

클래스 내부(body)는 strict mode로 실행된다. 성능 향상을 위해 엄격한 문법이 적용된다.

<br>

### Constructor

클래스로 생성된 객체의 인스턴스를 생성하고 초기화하기 위한 생성자 메소드이며 클래스 내부에 오직 하나의 `constructor()` 메소드만 존재할 수 있다.

`constructor()` 메소드는 인스턴스의 생성과 동시에 클래스 필드의 생성과 초기화를 진행한다.

`super` 키워드를 사용해 부모 클래스의 constructor를 호출할 수 있다.

<br>

  ### Getter and Setter

클래스의 필드를 참조하거나 조작하기 위해 사용하며 메소드 호출이 아닌 값을 할당하는 것으로 작동한다.

- getter : `get` 키워드를 사용하며, 반드시 반환하는 값이 있어야한다.
- setter : `set` 키워드를 사용하며, 클래스 필드의 값을 조작한다.

```javascript
class C {
    constructor(height) {
    	this.height = height;
    }

	get show() {
		return this.height;
	}

	set setHeight(height) {
		this.height = height;
	}
}

const c = new C(10);

c.show // 10, getter

c.setHeight = 30; // setter를 통한 필드 값 조작
c.show // 30
```
<br>

### Static methods

`static` 키워드를 사용해 메소드들을 정의할 수 있다. 이렇게 정의된 정적 메소드는 인스턴스가 아닌 이름으로 호출되며, 클래스의 인스턴스에서 호출 될 수 없다.

```javascript
class Point {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}

	static displayName = "Point";
	static distance(a, b) {
		const dx = a.x - b.x;
		const dy = a.y - b.y;

		return Math.hypot(dx, dy);	
	}	
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
p1.displayName; // undefined
p1.distance;    // undefined
p2.displayName; // undefined
p2.distance;    // undefined

console.log(Point.displayName);      // "Point"
console.log(Point.distance(p1, p2)); // 7.0710678118654755
```

<br>

### Sub classing with `extends`

`extends` 키워드를 사용해 클래스간의 상속 관계를 정의 할 수 있다. 각 클래스간의 상속 정의를 통해서 코드 재사용성을 높일 수 있다.

`super` 키워드를 사용해 부모 클래스의 constructor를 호출 할 수 있다.

```javascript
    class Animal {
    	constructor(name) {
	    this.name = name;
    	}

    	speak() {
	    console.log(`${this.name} makes a noise.`);
    	}
    }

    class Dog extends Animal {
    	constructor(name) {
		    super(name); // super class 생성자를 호출하여 name 매개변수 전달
	    }

	    speak() {
		    console.log(`${this.name} barks.`);
        }
    }

    let d = new Dog('Mitzie');
    d.speak(); // Mitzie barks.

```

<br><br>

## Factory Function

팩토리 함수(factory function)란 클래스나 생성자 함수는 아니지만 새로운 객체를 리턴하는 함수이다. 

클래스나 `new` 키워드의 복잡함 없이 객체 인스턴스를 생성 할 수 있다.

```javascript
const createUser = ({ userName, avatar }) => ({  
    userName, 
    avatar,  
    setUserName (userName) {  
      this.userName = userName;  
      return this;  
    }  
  });

  console.log(createUser({ userName: 'echo', avatar: 'echo.png' }));

  /*
  {  
    "avatar": "echo.png",  
    "userName": "echo",  
    "setUserName": [Function setUserName]  
  }  
  */

```

<br>

<br>


------

**Reference**
- [Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#defining_classes)
- [JavaScript Factory Functions with ES6+](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1)
