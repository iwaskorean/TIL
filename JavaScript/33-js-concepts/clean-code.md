# Clean Code

> ryanmcdermott의 **[clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)**를 번역한 포스트입니다.

깔끔하고 의미있는 코드(clean code)를 사용하면 가독성과 재사용성을 높이고 리팩토링하기 쉬운 더 나은 프로그램을 개발할 수 있다.

<br>

## Variables

#### 의미있고 발음하기 쉬운 변수 이름 사용

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



#### 검색 가능한 이름 사용

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



#### 불필요한 컨텍스트의 반복 지양

클래스나 객체의 이름이 무언가를 의미하고 있다면, 프로퍼티에 클래스나 객체의 이름을 반복할 필요 없다.

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



#### 함수는 한 가지만 수행

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



#### 함수의 기능을 설명하는 함수명

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



#### 함수는 오직 한 단계의 추상화여야 한다.

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



#### 중복된 코드 제거

중복된 코드들은 일부 코드의 로직을 변경할때 다른 코드들의 로직도 변경될 가능성이 있으므로 중복을 최소화 하는 것이 좋다.

중복된 코드를 제거한다는 것은 다양한 요소들을 처리 할 수 있는 함수/모듈/클래스 등의 추상화를  만드는 것을 의미한다. 추상화를 할때는 Classes 섹션에 나올 SOLID 원칙을 따라 올바르게 작성하는 것이 중요하다.

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



#### 플래그를 함수 매개변수로 사용하지말것

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



###### side effect part2 ...



<br>

<br>

------

**Reference**

- 
