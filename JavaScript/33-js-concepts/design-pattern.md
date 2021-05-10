# Design Pattern

디자인 패턴은 많은 개발자들이 개발하면서 겪은 다양한 경험과 시도들을 바탕으로 만들어진 구조화된 패턴이다. 이는 소프트웨어 개발에 있어서 발생하는 이슈들을 해결하는데 도움을 주는 재사용 가능한 솔루션 체계를 제공한다.

<br>

### Constructor Pattern

생성자 함수를 통해 동일한 객체의 여러 인스턴스를 만들 수 있다.

```javascript
// {}를 사용해 빈 객체 생성
let person = {};

// Object()를 사용해 빈 객체 생성
let person = new Object();

// 생성자 함수를 사용해 객체 생성
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.showName = () => console.log(this.name);
}
let person = new Person(‘Amy’, 28);

person.showName(); // Amy
```

<br>

### Prototype Pattern

객체 기반 생성 디자인 패턴이며 프로토타입에서 복제를 통해 새로운 인스턴스를 만든다. 새로운 객체를 직접 생성할때의 복잡함과 비효율성을 극복할 수 있는 대안이다.

```javascript
function Book(title, price) {
  this.title = title;
  this.price = price;
  this.printTitle = () => console.log(this.title);
}

function BookPrototype(prototype) {
  this.prototype = prototype;
  this.clone = () => {
    let book = new Book();
    book.title = prototype.title;
    book.price = prototype.price;
    
    return book;
  };
}

let sampleBook = new Book(‘JavaScript’, 15);
let prototype = new BookPrototype(sampleBook);
let book = prototype.clone();

book.printTitle();
```

자바스크립트에는 자체 내장적인(built-in) 프로토타입 기능이 있기 때문에 아래와 같은 방법으로 효율적인 프로토타입 패턴 적용이 가능하다.

```javascript
let book = {
  title: ‘JavaScript’,
  price: 15
}
let anotherBook = Object.assign({}, book);
```

<br>

### Command Pattern

커맨드 패턴의 주요 목적은 요청이나 작업들을 객체의 형태로 캡슐화하는것이다.

예시로, 이커머스 스토어의 결제 시스템에서 결제 수단에 따라 처리 할 특정 프로세스는 아래와 같다.

```
if (selectedPayment == ‘creditcard’) {
  // handle payment by creditcard
}
```

위와 같은 코드는 하나의 결제 수단에 대한 처리를 나타냈지만 다양한 결제 수단이 존재할 것이다. 위와 같은 코드를 사용할 경우 인터페이스가 아닌 구현된 부분만을 제공함으로 단단한 결합(tight coupling)이 발생한다.

커맨드 패턴은 이와 반대로 느슨한 결합(loose coupling)을 제공하는 훌륭한 솔루션이며 커맨드 패턴은 작업을 요청하는 코드와 실제 구현을 실행하는 부분을 분리해야한다.

```javascript
function Command(operation) {
  this.operation = operation;
}

Command.prototype.execute = function () {
  this.operation.execute();
}

function ProcessCreditCardPayment() {
  return {
    execute: function() {
      console.log(‘Credit Card’)
    }
  };
}

function ProcessPayPalPayment() {
  return {
    execute: function() {
      console.log(‘PayPal’)
    }
  };
}

function ProcessStripePayment() {
  return {
    execute: function() {
      console.log(‘Stripe’)
    }
  };
}

function CreditCardCommand() {
  return new Command(new ProcessCreditCardPayment());
}

function PayPalCommand() {
  return new Command(new ProcessPayPalPayment());
}

function StripeCommand() {
  return new Command(new ProcessStripePayment());
}

function PaymentSystem() {
  let paymentCommand;
  return {
    setPaymentCommand: function(command) {
      paymentCommand = command;
    },
    executeCommand: function() {
      paymentCommand.execute();
    }
  };
}

function run() {
  let paymentSystem = new PaymentSystem();
  paymentSystem.setPaymentCommand(new CreditCardCommand());
  paymentSystem.executeCommand();
  paymentSystem.setPaymentCommand(new PayPalCommand());
  paymentSystem.executeCommand();
  paymentSystem.setPaymentCommand(new StripeCommand());
  paymentSystem.executeCommand();
}

run();
```

<br>

### Observer Pattern

옵저버 패턴은 이벤트를 구독하는 구독 모델을 제공하며 해당 이벤트가 발생하면 알림을 받게 된다.

```javascript
function Newsletter() {
  this.observers = [];
}

// 구독 모델
Newsletter.prototype = {
  subscribe: function (observer) {
    this.observers.push(observer);
  },
  unsubscribe: function(observer) {
    this.observers = this.observers.filter(ob => ob !== observer);
  },
  notify: function() {
    this.observers.forEach(observer => console.log(‘Hello ‘ + observer.toString()));
  }
}

let subscriber1 = ‘Subscriber 1’;
let subscriber2 = ‘Subscriber 2’;

let newsletter = new Newsletter();

newsletter.subscribe(subscriber1);
newsletter.subscribe(subscriber2);
newsletter.notify();  // Hello Subscriber 1 Hello Subscriber 2

newsletter.unsubscribe(subscriber2);
newsletter.notify(); // Hello Subscriber 1
```

<br>

### Singleton Pattern

싱글톤 패턴은 가장 유명한 패턴 중 하나이며 클래스가 하나의 인스턴스만 가지도록 제한하고 전역적인 액세스를 가능하게 해주는 패턴이다. 이 패턴은 어플리케이션 전체에서 특정 부분에 대한 작업을 처리해야할 때 유용하다. 그러나 코드를 테스트하기 어렵다는 단점이 있으므로 사용시 주의해야한다.

```javascript
const utils = (function () {
  let instance;
  
  function initialize() {
    return {
      sum: function (a, b) {
        return a + b;
      }
    };
  }
  return {
    getInstance: function () {
      if (!instance) {
        instance = initialize();
      }
      return instance;
    }
  };
})();

let sum = utils.getInstance().sum(3, 5); // 8
```

<br>

### Module Pattern

모듈 패턴은 소프트웨어 모듈의 개념을 구현하는데 사용되는 패턴이다. 일반적으로 자주 사용되는 패턴이며 코드를 캡슐화하는 기능을 제공한다.

모듈 내에서 함수, 변수, 프로퍼티들을 public, private 멤버로 만들 수 있다.

```javascript
const bookModule = (function() {
  // private
  let title = ‘JavaScript’;
  let price = 15;
  // public
  return {
    printTitle: function () {
      console.log(title);
    }
  }
})();

bookModule.printTitle(); // JavaScript
```

<br>

### Factory Pattern

팩토리 패턴은 공장에서 제품을 생산하듯 비슷한 객체를 찍어내는 액션을 한다. 객체 생성을 나머지 코드로부터 분리하며 객체 생성 코드를 감싼 후  다른 객체를 생성할 수 있는 API를 노출 시킨다.

```javascript
const Vehicle = function() {};

const Car = function() {
  this.say = function() {
    console.log(‘I am a car’);
  }
};
const Truck = function() {
  this.say = function() {
    console.log(‘I am a truck’);
  }
};
const Bike = function() {
  this.say = function() {
    console.log(‘I am a bike’);
  }
};

const VehicleFactory = function() {
  this.createVehicle = (vehicleType) => {
    let vehicle;    switch (vehicleType) {
      case ‘car’:
        vehicle = new Car();
        break;
      case ‘truck’:
        vehicle = new Truck();
        break;
      case ‘bike’:
        vehicle = new Bike();
        break;
      default:
        vehicle = new Vehicle();
    }
 
    return vehicle;
  }
};

const vehicleFactory = new VehicleFactory();

let car = vehicleFactory.createVehicle(‘car’);
let truck = vehicleFactory.createVehicle(‘truck’);
let bike = vehicleFactory.createVehicle(‘bike’);

car.say(); // I am a car
truck.say(); // I am a truck
bike.say(); // I am a bike
```

<br>

------

**Reference**

- [7 JavaScript Design Patterns Every Developer Should Know](https://javascript.plainenglish.io/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf)
- [Learning JavaScript Design Patterns](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript)
