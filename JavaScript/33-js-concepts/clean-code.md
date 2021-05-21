# Clean Code

> *ryanmcdermott의 [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)를 번역한 포스트입니다.*

클린 코드(clean code)를 사용하면 코드의 가독성과 재사용성을 높일수 있으며 리팩토링하기 쉬운 프로그램을 개발할 수 있다.

<br>

## Table of Contents

- [Variables](#Variables)
- [Functions](#Functions)
- [Objects and Data Structures](#Objects-and-Data-Structures)
- [Classes](#Classes)
- [SOLID](#객체지향-5대-원칙-SOLID)

<br>

## Variables

#### 의미를 내포하고 있으며 발음하기 쉬운 변수 이름 사용

- Bad

  ```javascript
  const yyyymmdstr = moment().format("YYYY/MM/DD");
  ```

- Good

  ```javascript
  const currentDate = moment().format("YYYY/MM/DD");
  ```



#### 동일한 유형의 변수에 동일한 어휘 사용

- Bad

  ```javascript
  getUserInfo();
  getClientData();
  getCustomerRecord();
  ```

- Good

  ```javascript
  getUser();
  ```



#### 검색 가능한 이름 사용

작성된 코드의 리뷰나 검색을 위해서 프로그램을 쉽게 이해할 수 있는 의미를 담고 있는 변수명을 사용해야한다.

- Bad

  ```javascript
  // 86400000가 도대체 뭐임?!
  setTimeout(blastOff, 86400000);
  ```

- Good

  ```javascript
  // 대문자로 선언된 상수 이름을 통해 변수가 무엇을 의미하는지 알 수 있다.
  const MILLISECONDS_IN_A_DAY = 60 * 60 * 24 * 1000; //86400000;
  
  setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
  ```



#### 설명이 있는 변수명 사용

- Bad

  ```javascript
  const address = "One Infinite Loop, Cupertino 95014";
  const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
  saveCityZipCode(
    address.match(cityZipCodeRegex)[1],
    address.match(cityZipCodeRegex)[2]
  );
  ```

- Good

  ```javascript
  const address = "One Infinite Loop, Cupertino 95014";
  const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
  const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
  saveCityZipCode(city, zipCode);
  ```



#### 매핑시 명시적인 변수명 사용

명시적인(explicit)것이 암시적인(implicit)것보다 낫다.

- Bad

  ```javascript
  const locations = ["Austin", "New York", "San Francisco"];
  locations.forEach(l => {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // 1이 도대체 뭐임?!
    dispatch(l);
  });
  ```

- Good

  ```javascript
  const locations = ["Austin", "New York", "San Francisco"];
  locations.forEach(location => {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch(location);
  });
  ```



#### 불필요한 컨텍스트의 반복 지양

클래스나 객체의 이름이 무언가를 의미하고 있다면, 프로퍼티에 클래스/객체의 이름을 반복할 필요 없다.

- Bad

  ```javascript
  const Car = {
    carMake: "Honda",
    carModel: "Accord",
    carColor: "Blue"
  };
  
  function paintCar(car) {
    car.carColor = "Red";
  }
  ```

- Good

  ```javascript
  const Car = {
    make: "Honda",
    model: "Accord",
    color: "Blue"
  };
  
  function paintCar(car) {
    car.color = "Red";
  }
  ```



#### 함수 인자에 default 값 사용

- Bad

  ```javascript
  function createMicrobrewery(name) {
    const breweryName = name || "Hipster Brew Co.";
    // ...
  }
  ```

- Good

  ```javascript
  function createMicrobrewery(name = "Hipster Brew Co.") {
    // ...
  }
  ```

<br>

## Functions

#### 하나 또는 두 개의 함수 인자 사용

두 개의 인자가 가장 이상적이며 함수의 인자의 개수를 최소화하면 함수를 더 쉽게 테스트할 수 있다. 인자가 많을경우 테스트 케이스가 많아져 비효율적이다.

- Bad

  ```javascript
  function createMenu(title, body, buttonText, cancellable) {
    // ...
  }
  
  createMenu("Foo", "Bar", "Baz", true);
  ```

- Good

  ```javascript
  function createMenu({ title, body, buttonText, cancellable }) {
    // ...
  }
  
  createMenu({
    title: "Foo",
    body: "Bar",
    buttonText: "Baz",
    cancellable: true
  });
  ```



#### 함수는 한 가지만 수행

두 가지 이상의 일을 수행하는 함수들은 비교하거나 테스트할때의 어려움이 있다. 함수를 하나의 작업으로 분리할 수 있으면 쉽게 리팩토링할 수 있으며 가독성을 높일 수 있다.

- Bad

  ```javascript
  function emailClients(clients) {
    clients.forEach(client => {
      const clientRecord = database.lookup(client);
      if (clientRecord.isActive()) {
        email(client);
      }
    });
  }
  ```

- Good

  ```javascript
  function emailActiveClients(clients) {
    clients.filter(isActiveClient).forEach(email);
  }
  
  function isActiveClient(client) {
    const clientRecord = database.lookup(client);
    return clientRecord.isActive();
  }
  ```



#### 함수의 기능을 설명하는 함수명

- Bad

  ```javascript
  function addToDate(date, month) {
    // ...
  }
  
  const date = new Date();
  
  // 연, 월, 일 중 무엇을 더하는 함수인가 알기 어려움
  addToDate(date, 1);
  ```

- Good

  ```javascript
  function addMonthToDate(month, date) {
    // ...
  }
  
  const date = new Date();
  addMonthToDate(1, date);
  ```



#### 함수는 오직 한 단계의 추상화여야 한다.

두 단계 이상의 추상화가 적용된 경우 많은 작업을 수행함으로 비효율적이다.

- Bad

  ```javascript
  function parseBetterJSAlternative(code) {
    const REGEXES = [
      // ...
    ];
  
    const statements = code.split(" ");
    const tokens = [];
    REGEXES.forEach(REGEX => {
      statements.forEach(statement => {
        // ...
      });
    });
  
    const ast = [];
    tokens.forEach(token => {
      // lex...
    });
  
    ast.forEach(node => {
      // parse...
    });
  }
  ```

- Good

  ```javascript
  function parseBetterJSAlternative(code) {
    const tokens = tokenize(code);
    const syntaxTree = parse(tokens);
    syntaxTree.forEach(node => {
      // parse...
    });
  }
  
  function tokenize(code) {
    const REGEXES = [
      // ...
    ];
  
    const statements = code.split(" ");
    const tokens = [];
    REGEXES.forEach(REGEX => {
      statements.forEach(statement => {
        tokens.push(/* ... */);
      });
    });
  
    return tokens;
  }
  
  function parse(tokens) {
    const syntaxTree = [];
    tokens.forEach(token => {
      syntaxTree.push(/* ... */);
    });
  
    return syntaxTree;
  }
  ```



#### 중복된 코드 제거

중복된 코드들은 일부 코드의 로직을 변경할때 다른 코드들의 로직도 변경될 가능성이 있으므로 중복을 최소화 하는 것이 좋다.

중복된 코드를 제거한다는 것은 다양한 요소들을 처리 할 수 있는 함수/모듈/클래스 등의 추상화를  만드는 것을 의미한다. 추상화를 할때는 Classes 섹션에 나올 SOLID 원칙을 따라 올바르게 작성하는 것이 중요하다.

- Bad

  ```javascript
  function showDeveloperList(developers) {
    developers.forEach(developer => {
      const expectedSalary = developer.calculateExpectedSalary();
      const experience = developer.getExperience();
      const githubLink = developer.getGithubLink();
      const data = {
        expectedSalary,
        experience,
        githubLink
      };
  
      render(data);
    });
  }
  
  function showManagerList(managers) {
    managers.forEach(manager => {
      const expectedSalary = manager.calculateExpectedSalary();
      const experience = manager.getExperience();
      const portfolio = manager.getMBAProjects();
      const data = {
        expectedSalary,
        experience,
        portfolio
      };
  
      render(data);
    });
  }
  
  ```

- Good

  ```javascript
  function showEmployeeList(employees) {
    employees.forEach(employee => {
      const expectedSalary = employee.calculateExpectedSalary();
      const experience = employee.getExperience();
  
      const data = {
        expectedSalary,
        experience
      };
  
      switch (employee.type) {
        case "manager":
          data.portfolio = employee.getMBAProjects();
          break;
        case "developer":
          data.githubLink = employee.getGithubLink();
          break;
      }
  
      render(data);
    });
  }
  
  ```



#### Object.assign을 사용한 기본 객체 설정

- Bad

  ```javascript
  const menuConfig = {
    title: null,
    body: "Bar",
    buttonText: null,
    cancellable: true
  };
  
  function createMenu(config) {
    config.title = config.title || "Foo";
    config.body = config.body || "Bar";
    config.buttonText = config.buttonText || "Baz";
    config.cancellable =
      config.cancellable !== undefined ? config.cancellable : true;
  }
  
  createMenu(menuConfig);
  ```

- Good

  ```javascript
  const menuConfig = {
    title: "Order",
    // User did not include 'body' key
    buttonText: "Send",
    cancellable: true
  };
  
  function createMenu(config) {
    let finalConfig = Object.assign(
      {
        title: "Foo",
        body: "Bar",
        buttonText: "Baz",
        cancellable: true
      },
      config
    );
    return finalConfig
    // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
    // ...
  }
  
  createMenu(menuConfig);
  ```



#### 플래그를 함수 매개변수로 사용하지말것

플래그 매개변수를 사용해 조건 분기를 통해 함수가 다른 동작을 수행하는 경우 함수를 분리시켜야 한다.

- Bad

  ```javascript
  function createFile(name, temp) {
    if (temp) {
      fs.create(`./temp/${name}`);
    } else {
      fs.create(name);
    }
  }
  ```

- Good

  ```javascript
  function createFile(name) {
    fs.create(name);
  }
  
  function createTempFile(name) {
    createFile(`./temp/${name}`);
  }
  ```



#### Side Effect 방지(part 1)

함수는 값을 가져와 다른 값으로 반환하는 것 이외의 작업을 수행하면 Side effect가 발생한다. 이를 방지하기 위해서 구조 없이 객체의 상태를 공유, 어떤것으로든 기록될 수 있는 가변적인 데이터 타입들의 사용, Side effect가 발생하는 곳의 비집중화 등을 피해야한다.

- Bad

  ```javascript
  // 만약 이 name을 사용하는 다른 함수가 있어 이 변수가 공유되면, 의도치않게 변수의 값이 변할 수 있다.
  
  let name = "Ryan McDermott";
  
  function splitIntoFirstAndLastName() {
    name = name.split(" ");
  }
  
  splitIntoFirstAndLastName();
  
  console.log(name); // ['Ryan', 'McDermott'];
  ```

- Good

  ```javascript
  function splitIntoFirstAndLastName(name) {
    return name.split(" ");
  }
  
  const name = "Ryan McDermott";
  const newName = splitIntoFirstAndLastName(name);
  
  console.log(name); // 'Ryan McDermott';
  console.log(newName); // ['Ryan', 'McDermott'];
  ```



#### Side Effect 방지(part 2)

자바스크립트에서 일부 값들은 변경이 불가하며(immutable) 다른 일부 값들은 변경이 가능하다(mutable). 객체와 배열 두 종류는 변경 가능한 값이므로 함수에 매개변수로 전달할때 주의해야한다. 자바스크립트 함수는 객체의 프로퍼티와 배열의 요소의 값을 바꿀 수 있으며 이것이 버그의 원인이 된다.

- Bad

  ```javascript
  const addItemToCart = (cart, item) => {
    cart.push({ item, date: Date.now() });
  };
  // 쇼핑카트에 추가되어있던 item에 대한 배열(cart)은 반환되지 않는다.
  ```

- Good

  ```javascript
  const addItemToCart = (cart, item) => {
    return [...cart, { item, date: Date.now() }];
  };
  // addItemToCart가 반환할때 cart를 복제 및 새로운 item을 추가한 후 반환하기 때문에 이전 쇼핑카트에 추가해놓은 아이템에 영향을 주지 않는다.
  ```



#### 전역 함수에 쓰지말 것

전역 범위를 작성하는 것은(polluting globals)은 나쁜 습관이다. 다른 라이브러리와 충돌할 가능성이 있고 API 사용자가 프로덕션시 예외가 발생할 때까지 모를수도 있다. 예를 들어 Array 메소드를 확장해 두 배열간의 차이를 나타낼수 있는 `diff` 메소드를 만들려고 할때, `Array.prototype`을 사용할 경우 다른 라이브러리와 충돌할 가능성이 있다. 이 경우 `Array.prototype`이 아닌 ES6의 class 키워드를 사용할 수 있다.

- Bad

  ```javascript
  Array.prototype.diff = function diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  };
  ```

- Good

  ```javascript
  class SuperArray extends Array {
    diff(comparisonArray) {
      const hash = new Set(comparisonArray);
      return this.filter(elem => !hash.has(elem));
    }
  }
  ```



#### 명령형 프로그래밍보다 함수형 프로그래밍을 지향

- Bad

  ```javascript
  const programmerOutput = [
    {
      name: "Uncle Bobby",
      linesOfCode: 500
    },
    {
      name: "Suzie Q",
      linesOfCode: 1500
    },
    {
      name: "Jimmy Gosling",
      linesOfCode: 150
    },
    {
      name: "Gracie Hopper",
      linesOfCode: 1000
    }
  ];
  
  let totalOutput = 0;
  
  for (let i = 0; i < programmerOutput.length; i++) {
    totalOutput += programmerOutput[i].linesOfCode;
  }
  ```

- Good

  ```javascript
  const programmerOutput = [
    {
      name: "Uncle Bobby",
      linesOfCode: 500
    },
    {
      name: "Suzie Q",
      linesOfCode: 1500
    },
    {
      name: "Jimmy Gosling",
      linesOfCode: 150
    },
    {
      name: "Gracie Hopper",
      linesOfCode: 1000
    }
  ];
  
  const totalOutput = programmerOutput.reduce(
    (totalLines, output) => totalLines + output.linesOfCode,
    0
  );
  ```



#### 조건문 캡슐화

- Bad

  ```javascript
  if (fsm.state === "fetching" && isEmpty(listNode)) {
    // ...
  }
  ```

- Good

  ```javascript
  function shouldShowSpinner(fsm, listNode) {
    return fsm.state === "fetching" && isEmpty(listNode);
  }
  
  if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
    // ...
  }
  ```



#### 부정연산자를 사용한 조건문 지양

- Bad

  ```javascript
  function isDOMNodeNotPresent(node) {
    // ...
  }
  if (!isDOMNodeNotPresent(node)) {
    // ...
  }
  ```

- Good

  ```javascript
  function isDOMNodePresent(node) {
    // ...
  }
  if (isDOMNodePresent(node)) {
    // ...
  }
  ```



#### 다수의 조건문 사용 지양 

다수의 조건문이 필요한 경우 다형성 즉, class 상속(extend)을 이용하는것이 좋다. 

- Bad

  ```javascript
  class Airplane {
    // ...
    getCruisingAltitude() {
      switch (this.type) {
        case "777":
          return this.getMaxAltitude() - this.getPassengerCount();
        case "Air Force One":
          return this.getMaxAltitude();
        case "Cessna":
          return this.getMaxAltitude() - this.getFuelExpenditure();
      }
    }
  }
  ```

- Good

  ```javascript
  class Airplane {
    // ...
  }
  
  class Boeing777 extends Airplane {
    // ...
    getCruisingAltitude() {
      return this.getMaxAltitude() - this.getPassengerCount();
    }
  }
  
  class AirForceOne extends Airplane {
    // ...
    getCruisingAltitude() {
      return this.getMaxAltitude();
    }
  }
  
  class Cessna extends Airplane {
    // ...
    getCruisingAltitude() {
      return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
  ```



#### 타입 검사 피하기(part 1)

자바스크립트는 인자에 어떠한 타입도 사용할 수 있는 untyped 언어이다. 이로인해 타입 검사를 해야하는 경우가 생길수도 있다. 이를 피하기 위한 많은 방법 중 가장 먼저 고려해야할 방법은 일관된 API를 만드는 것이다.

- Bad

  ```javascript
  function travelToTexas(vehicle) {
    if (vehicle instanceof Bicycle) {
      vehicle.pedal(this.currentLocation, new Location("texas"));
    } else if (vehicle instanceof Car) {
      vehicle.drive(this.currentLocation, new Location("texas"));
    }
  }
  ```

- Good

  ```javascript
  function travelToTexas(vehicle) {
    vehicle.move(this.currentLocation, new Location("texas"));
  }
  ```



#### 타입 검사 피하기(part 2)

문자열이나 정수형 같은 기본 원시적인 타입의 값들로 작업을한다면 다형성을 사용할 수 없다. 이 경우 타입 검사가 필요하다면 타입스크립트를 사용하는 것을 고려해야한다. 타입스크립트는 자바스크립트 구문에 정적 타이핑을 제공하는 훌륭한 대안이다.

- Bad

  ```javascript
  function combine(val1, val2) {
    if (
      (typeof val1 === "number" && typeof val2 === "number") ||
      (typeof val1 === "string" && typeof val2 === "string")
    ) {
      return val1 + val2;
    }
  
    throw new Error("Must be of type String or Number");
  }
  ```

- Good

  ```javascript
  function combine(val1, val2) {
    return val1 + val2;
  }
  ```



#### 지나친 최적화 금지

모던 브라우저들은 런타임에 보이진 않지만 내부적으로(under-the-hood)  많은 최적화를 수행한다. 따라서 필요이상의 지나친 최적화는 시간 낭비이다.

- Bad

  ```javascript
  // On old browsers, each iteration with uncached `list.length` would be costly
  // because of `list.length` recomputation. In modern browsers, this is optimized.
  for (let i = 0, len = list.length; i < len; i++) {
    // ...
  }
  ```

- Good

  ```javascript
  for (let i = 0; i < list.length; i++) {
    // ...
  }
  ```



#### 필요없는 코드 삭제

더이상 사용하지않는 코드(dead code)들은 중복 코드들만큼 나쁘며 코드베이스에 저장할 이유가 없다. 호출되지않는 코드들을 제거해야한다.

- Bad

  ```javascript
  function oldRequestModule(url) {
    // ...
  }
  
  function newRequestModule(url) {
    // ...
  }
  
  const req = newRequestModule;
  inventoryTracker("apples", req, "www.inventory-awesome.io");
  ```

- Good

  ```javascript
  function newRequestModule(url) {
    // ...
  }
  
  const req = newRequestModule;
  inventoryTracker("apples", req, "www.inventory-awesome.io");
  ```

<br>

## Objects and Data Structures 

#### getter와 setter를 사용해라

getter와 setter를 사용해 객체를 액세스하는 것이 객체의 프로퍼티를 찾는것보다 더 낫다.  그 이유는 아래와 같다.

1. 객체의 속성을 얻는 것 이상의 작업이 필요할때 모든 접근자들을 변경할 필요가 없다.
2. `set`을 사용해 쉽게 유효성 검증 절차를 만들 수 있다.
3. 내부적으로 캡슐화가 가능하다.
4. 코드 로깅과 에러 처리가 간편하다.
5. 서버에서부터 객체의 프로퍼티를 가져올때 lazy load가 가능하다.

- Bad


```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

- Good


```javascript
function makeBankAccount() {
  // balance는 private한 멤버 변수이다.
  let balance = 0;

  // "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```



#### 객체에 private 멤버를 만들어라

클로저(ES5 이하)를 사용해 private한 멤버 변수를 만들 수 있다.

- Bad

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

- Good

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

<br>

## Classes

#### ES5의 함수보다 ES6의 클래스를 사용해라

기존 ES5 클래스에서는 클래스 상속, 생성자 메소드를 구현하는 것이 어렵다. 만약 상속이 필요한 경우 ES6의 클래스를 사용하는 것이 좋다. 그러나 더 크고 복잡한 객체가 필요하다면 클래스보다 작은 함수를 사용하는 것이 더 낫다.

- Bad

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

- Good

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```



#### 메소드 체이닝을 사용해라

메소드 체이닝은 자바스크립트에서 매우 유용한 패턴이며 제이쿼리나 lodash 같은 많은 라이브러리에서 사용된다. 메소드 체이닝을 사용하면 코드를 잘 표현할 수 있으며 간결하게 만들어준다. 클래스 함수에서 간단히 모든 함수에 `this`를 반환하는것만으로  클래스 메소드를 연결할 수 있다.

- Bad

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

- Good

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // 체이닝을 위한 this 반환
    return this;
  }

  setModel(model) {
    this.model = model;
    // 체이닝을 위한 this 반환
    return this;
  }

  setColor(color) {
    this.color = color;
    // 체이닝을 위한 this 반환
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // 체이닝을 위한 this 반환
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```



#### 상속보다는 컴포지션을 사용해라

상속을 사용했을때보다 컴포지션을 사용할때의 이득이 많기 때문에 가능하다면 상속보다는 컴포지션을 사용해야 한다. ([Design pattern](https://en.wikipedia.org/wiki/Design_Patterns) by Gang of Four) 

그러나 상속의 관계가 "has-a"(사용자 -> 사용자정보)가 아닌 "is-a" (사람 -> 동물)일때, 기반이 되는 베이스 클래스를 재사용할 수 있으며 베이스 클래스를 수정해 파생된 클래스들을 수정하고 싶을때는 상속을 사용하는것이 더 낫다.

- Bad

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Employees가 tax data를 가지고 있기 때문에 이 클래스는 좋지않다.
// EmployeeTaxData는 Employee 타입이 아니다.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

- Good

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

<br>

## 객체지향 5대 원칙 SOLID

#### 단일 책임 원칙(SRP, Single Responsibility Principle)

클린 코드에서는 클래스를 수정하는 이유가 한 가지를 넘으면 안된다고 말한다. 이것은 하나의 클래스의 너무 많은 기능을 가득 채우는 것(jam-pack)과 같다. 이 문제는 클래스가 개념적으로 구성되어있지 않으며 클래스를 바꿔야할 많은 이유가 된다. 하나의 클래스에 너무 많은 기능들이 있고 이러한 작은 기능들을 수정이 다른 모듈들에 어떤 영향을 끼치지는지 알기 어렵기 때문에 클래스의 수정 시간을 최소화 하는것이 중요하다.

- Bad

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

- Good

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```



#### 개방/폐쇄 원칙(OCP, Open/Closed Principle)

Bertrand Meyer에 의하면 소프트웨어 엔터티들(클래스, 모듈, 함수 등)은 확장을 위해서 개방적이어야하며 수정시에는 폐쇄적이어야 한다. 이 원칙은 기본적으로 존재하는 코드의 수정 없이  사용자가 새로운 기능들을 추가할 수 있도록 허용해야한다는 것을 의미한다.

- Bad

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

- Good

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```



#### 리스코프 치환 원칙(LSP, Liskov Substitution Principle)

리스코프 치환 원칙은 매우 간단하지만 강력한 개념이다. 이 원칙의 정의는  "만약 S가 T의 하위 타입(sub type)이면,  프로그램 속성의 변경 없이 T 타입의 객체는  S 타입의 객체로 치환될 수 있다. "는 것이다.

예를 들어, 부모 클래스와 자식 클래스가 있는 경우 베이스 클래스와 자식 클래스는 잘못된 결과 없이 교환해서 사용할 수 있다.  다른 예로 수학적으로 정사각형은 직사각형이지만 상속을 통해 "is-a" 관계로 모델링할 경우 문제가 발생한다.

- Bad

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

- Good

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```



#### 인터페이스 분리 원칙(ISP, Interface Segregation Principle)

자바스크립트에는 인터페이스가 없기 때문에  다른 원칙들 만큼 엄격하게 적용되진 않지만 타입 시스템이 없더라도 중요하고 관련있는 원칙이다.

인터페이스 분리 원칙은 "클라이언트가 사용하지 않는 인터페이스에 의존하도록 강요해서는 안된다." 규정하고 있으며 인터페이스들은 덕타이핑 때문에  암시적인 contract이다.

인터페이스 분리 원칙의 좋은 예는 많은 양의 설정 객체(large settings objects)가 필요한 클래스이다. 대부분 모든 설정들을 세팅을 요구하는것이 아니기때문에 클라이언트가 많은 양의 옵션을 선택할 필요가 없다. 선택적인 설정을 통해 fat 인터페이스를 만드는 것을 방지할 수 있다.

- Bad

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

- Good

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```



#### 의존관계 역전 원칙(DIP, Dependency Inversion Principle)

이 원칙은 두가지의 필수 요소를 가지고 있다.

1. 상위 레벨의 모듈은 하위 레벨의 모듈에 의존해서는 안된다. 두 모듈 다 추상화에 의존해야 한다.
2. 추상화는 세부사항에 의존해서는 안된다. 세부사항은 추상화에 의해 달라져야한다.

자바스크립트에서는 인터페이스가 없기때문에 추상화에 의존하는것은 암시적인 contract다. 즉, 다른 객체와 클래스에 노출되는 메소드와 프로퍼티들이 암시적인 contract가 된다.  아래의 예제 코드에서의 암시적인 contract는 `InventoryTracker`에 대한 요청 모듈들이 `requestItems` 메소드를 가지게 된다는 것이다.

- Bad

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

- Good

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

 <br>

<br>

###### .. Testing, Concurrency, Error Handling, Formatting, Commentsm, Translation

------

**Reference**

- [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
