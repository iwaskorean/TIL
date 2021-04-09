# Prototype Inheritance and Prototype Chain

<br>

### Problem with creating object with the constructor function

```
function Human(firstName, lastName) {
	this.firstName = firstName,
	this.lastName = lastName,
	this.fullName = function() {
		return this.firstName + " " + this.lastName;
	}
}

const person1 = new Human("Daeho", "Lee");
const person2 = new Human("Junwoo", "Jeon");
```

- `Human` 생성자 함수를 사용해서 `person1`, `person2` 객체를 생성하면 자바스크립트 엔진은 `person1`과 `person2`를 위한 생성자 함수의 복사본을 생성한다.

  ![img](https://miro.medium.com/max/1050/1*3ftgUPoTz5nSmGPwTjf7VA.png)

- 생성자 함수를 사용해 생성된 모든 객체는 고유한 프로퍼티와 메소드의 복사본을 갖게 된다. 즉, 각 객체에 대한 함수의 인스턴스를 개별적으로 저장하고 이것은 메모리 낭비로 이어진다.

- 프로토타입을 이용하면 이러한 메모리 낭비를 막을 수 있다.

<br> <br>

### [[Prototype]]

![img](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/resource/yongkwan/17/01.png)


- 자바스크립트에서 객체들은 `[[Prototype]]` 이라는 숨겨진 프로퍼티를 갖는다. 이 프로퍼티는 `null `값을 가지거나 다른 객체를 참조하는데 `[[Prototype]]` 프로퍼티에 의해 참조 대상이 되는 객체를 프로토타입이라고 한다.

- 우리가 `object`에 있는 프로퍼티를 읽을려고 할때 프로퍼티가 없을 경우 자바스크립트는 자동으로 프로토타입에서 프로퍼티를 찾는다. 이것을 프로토타입 상속(prototypal inheritance) 이라고 부른다.

- `[[Prototype]]` 프로퍼티는 내부의 숨겨진 프로퍼티지만 `__proto__`를 사용하면 값을 설정 할 수 있다. 즉 `__proto__`는 `[[Prototype]]`의 getter/setter 인 것이다. 

- 최근에는 `__proto__` 대신  `Object.getPrototypeOf`나  `Object.setPrototypeOf`을 사용해 프로토타입을 get하거나 set한다.

  ```
  let animal = {
    eats: true
  };
  let rabbit = {
    jumps: true
  };
  
  rabbit.__proto__ = animal; // sets rabbit.[[Protytpe]] = animal
  
  // __proto__로 rabbit의 프로토타입을 animal로 set 했기 때문에
  // rabbit에서 eats와 jumps 프로퍼티 모두 찾을 수 있다.
  alert( rabbit.eats ); // true (**)
  alert( rabbit.jumps ); // true
  ```



- 프로토타입 체인은 더 길어질 수도 있다.

  ![img](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/resource/yongkwan/17/04.png)

  ```
  let animal = {
    eats: true,
    walk() {
      alert("Animal walk");
    }
  };
  
  let rabbit = {
    jumps: true,
    __proto__: animal // rabbit의 프로토타입을 animal로 set
  };
  
  let longEar = {
    earLength: 10,
    __proto__: rabbit // longEar의 프로토타입을 rabbit으로 set
  };
  
  
  longEar.walk(); // Animal walk
  alert(longEar.jumps); // true (from rabbit)
  ```

  - 참조는 순환될 수 없다.  `__proto__`가 순환하도록 할 경우 자바스크립트는 에러를 보낼것이다.

  - `__proto__`의 값은 `null` 또는 다른 객체만 사용 될 수 있으며 다른 타입들은 무시된다.

  - 객체는 두 개의 객체를 상속받지 못하고 오직 하나의 `[[Prototype]]`만 가질 수 있다.

<br>

<br>

### Writing doesn't use prototype

- 프로토타입은 오직 프로퍼티를 읽을때만 사용하며 수정하거나 지우는 작업은 객체에 직접 적용된다.

  ![img](https://miro.medium.com/max/394/1*GZ90h9i3PsPSOAP7jCGn8g.png)

  ```
  let animal = {
    eats: true,
    walk() {
      /* this method won't be used by rabbit */
    }
  };
  
  let rabbit = {
    __proto__: animal
  };
  
  rabbit.walk = function() {
    alert("Rabbit! Bounce-bounce!");
  };
  
  rabbit.walk(); // Rabbit! Bounce-bounce!
  ```

  - `rabbit.walk()`를 호출하면 프로토타입의 메소드가 아닌 객체 `rabbit`에 있는 메소드를 찾아 실행한다.

<br>

<br>

### value of "this"

- 객체에서 호출한 메소드, 프로토타입에서 호출한 메소드 관계없이 `this`는 언제나 `.`(dot) 왼쪽에 있는 객체이다.

- 많은 메소드를 가지고 있는 큰 객체를 만들고 여러 객체가 이 큰 객체를 상속받게 하는 경우 상속받은 메소드를 사용하게 되더라도 객체는 프로토타입이 아닌 자신의 객체를 변경한다.

  ![img](https://ko.javascript.info/article/prototype-inheritance/proto-animal-rabbit-walk-3.svg)

  ```
  // animal has methods
  let animal = {
    walk() {
      if (!this.isSleeping) {
        alert(`I walk`);
      }
    },
    sleep() {
      this.isSleeping = true;
    }
  };
  
  let rabbit = {
    name: "White Rabbit",
    __proto__: animal
  };
  
  // modifies rabbit.isSleeping
  rabbit.sleep();
  
  alert(rabbit.isSleeping); // true
  alert(animal.isSleeping); // undefined (no such property in the prototype)
  ```

  - 상속받은 메소드의 `this`는 `animal`이 아닌 호출될때 `.` 왼쪽에 있는 객체가 된다. 따라서 `this`에 데이터를 쓰면 해당 객체에 저장된다.

  - 결과적으로, 메소드는 공유 되지만 객체의 상태는 공유되지 않는다.

<br>

<br>

### constructor and prototype property

- 모든 함수는 기본적으로 `"prototype"` 프로퍼티를 갖는다.

- `"prototype"` 프로퍼티는 `constructor` 프로퍼티를 가지고 있는 객체를 가르키며 `constructor` 프로퍼티는 함수 자신을 가르킨다.

  ```
  function Rabbit() {}
  // by default:
  // Rabbit.prototype = { constructor: Rabbit }
  
  alert( Rabbit.prototype.constructor == Rabbit ); // true
  ```

- `constructor` 프로퍼티는 `[[Prototype]]`을 통해 모든 `Rabbit` 객체에서 사용 할 수 있다.

  ​	![img](https://ko.javascript.info/article/function-prototype/rabbit-prototype-constructor.svg)

  ```
  function Rabbit() {}
  // by default:
  // Rabbit.prototype = { constructor: Rabbit }
  
  let rabbit = new Rabbit(); // inherits from {constructor: Rabbit}
  
  alert(rabbit.constructor == Rabbit); // true (from prototype)
  ```

- 기존 객체의 `constructor` 프로퍼티를 사용해 새로운 객체를 만들 수도 있다.

  ```
  function Rabbit(name) {
    this.name = name;
    alert(name);
  }
  
  let rabbit = new Rabbit("White Rabbit");
  
  let rabbit2 = new rabbit.constructor("Black Rabbit");
  ```

<br>

<br>

###### Note

######  - 상속 내용 추가해서 정리할 것

------

**Reference**

- [Prototype inheritance](https://javascript.info/prototype-inheritance)
- [F.prototype](https://javascript.info/function-prototype)