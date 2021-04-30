# Recursion

자바스크립트에서의 **재귀(recursion)**란 함수가 자기 자신을 호출하는 기능이다.

<br>

### Key Components

모든 재귀 함수에는 두 개의 필수 요소와 자주 사용되는 하나의 요소가 있다.

1. 종료 조건(Termination Condition)

   - 항상 필요로하는 구성 요소는 아니며, 잘못된 입력으로부터 보호하기 위한 코드를 의미한다.

   - 주어진 입력이 잘못된 결과 값을 야기할 경우 재귀가 실행되기전에 함수를 종료한다.

2. 베이스 케이스(Base Case)

   - 베이스 케이스는 재귀 함수를 중지하므로 종료 조건과 유사하지만 종료 조건은 잘못된 입력에 대한 안전 처리(fail-safe)지만, 베이스 케이스는 재귀 함수의 실제 목표이다.

3. 재귀(The Recursion)

   - 함수 자체를 호출하는 구성 요소.  함수가 자기 자신을 호출할때 무한루프에 빠지지 않도록 새 호출 함수의 파라미터들은 계속해서 변경되어야한다. 

<br>

### Examples

1. ##### 무한 재귀(Infinite Recursion)

   ```javascript
   function func() {
   	func();
   }
   
   func();
   ```

   - 이러한 형태의 재귀 함수는 페이지나 브라우저가 충돌할때까지 무한히 실행된다.

     

2. #####  팩토리얼(Factorial)

   ```javascript
   function factorial(n) {
   	if(n < 0) {
   		return;
   	} // 종료 조건
   	
   	if(n === 0) {
   		return 1;
   	} // 베이스 케이스
   	
   	return n * factorial(n-1); // 재귀
   }
   
   factorial(4); // 24
   ```

   ```
   factorial(4) returns 4 * factorial(3)
   factorial(3) returns 3 * factorial(2)
   factorial(2) returns 2 * factorial(1)
   factorial(1) returns 1 * factorial(0)
   factorial(0) returns 1
   
   // 베이스 케이스를 만족, 따라서 재귀 함수는 안에서부터 바깥으로 리턴한다.
   
   factorial(0) => 1
   factorial(1) => 1 * 1
   factorial(2) => 2 * 1 * 1
   factorial(3) => 3 * 2 * 1 * 1
   factorial(4) => 4* 3 * 2 * 1 * 1 // 24
   ```

   

3. ##### 종료 조건 설정(Setting Up A Leave Event)

   - 재귀 함수를 구성할때는 재귀 루프를 끝낼수 있는 종료 조건(Leave Event, Termination Condition)이 있어야한다.

   ```javascript
   function countdown(n) {
     console.log(n); // 
     
     if(n >= 2) countdown(n-1); // termination condition
   }
   countdown(3);
   // 3
   // 2
   // 1
   ```

   

4. ##### 재귀 순회(Recursive Traversal)

   - 재귀를 이용해 HTML 노드를 크롤링 할 수 있다.

   ```html
   <div id="start">
     <div id="inner">Check The Console</div>
     <span>To See The Recursive</span>
     <div id="outer">Traversal</div>
   </div>
   ```

   ```javascript
   function Traverse (ele, callback) { 
     callback (ele); 
     ele = ele.firstChild; 
     while (ele) { 
       Traverse (ele, callback); 
       ele = ele.nextSibling; 
     } 
   }
   
   Traverse(document.getElementById('start'), (ele) => console.log(ele));
   
   // ### console
   //<div id="start"> ... </div>
   // #text
   //<div id="inner"> ... <div>
   // "Check The Console"
   //<span> ... <span>
   // "To See The Recursive"
   //<span> ... <span>
   // "To See The Recursive"
   // <div id="outer"> ... </div>
   // "Traversal"
   // #text
   ```

<br>

<br>


------

**Reference**

- [Intro to Recursion](https://medium.com/@newmanbradm/intro-to-recursion-984a8bd50f4b)
- [Understanding Recursion in JavaScript](https://medium.com/@zfrisch/understanding-recursion-in-javascript-992e96449e03)
