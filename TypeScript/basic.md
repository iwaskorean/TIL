# Basic

### TypeScript

타입스크립트는 정적 타입 언어이며 자바스크립트에 타입 시스템을 적용해 안정성을 보장한다.

타입 시스템은 다음과 같은 기능을 한다.

- 에러를 catch 하는 것을 도와준다.
- Type annotation을 통해 프로퍼티들에 대한 묘사와 코멘트 제공
- 코드 작성시에만 active

**Process**

- TypeScript code → TypeScript complier→ Plain old JavaScript
- 브라우저에서는 타입스크립트가 아닌 자바스크립트로 실행한다.

<br>

### Type

타입이란 프로퍼티와 값들을 가지고 있는 함수를 나타내기 위한 방식이다.

원시 타입은 다음과 같다.

-  `number`, `boolean`, `void`, `undefined`, `string`, `symbol`, `null`

객체 타입은 다음과 같다.

- `functions`, `arrays`, `classes`, `objects`

**그렇다면 왜 타입에 신경써야할까?** 

> Why do we care about types?

그 이유는 다음과 같다.

- 타입들은 타입스크립트 컴파일러가 에러를 위해 우리가 작성한 코드를 분석할때 사용되기 때문이다.

- 타입은 다른 사람이 작성한 코드의 이해를 도울 수 있다.

  <br>

타입스크립트에서의 타입은 사용될 수 있는 코드 모든 곳에서 사용되어야한다.

<br>

## Type Inference and Type Annotation

### Type Inference

> Typescript tries to figure out what type of value a variable refers to

type infrenece란 타입스크립트가 자동으로 타입을 추측하는 것이다. 

**When to use ?** Always!

<br>

### Type Annotation

> Code we add to tell Typescript what type of value a variable will refer to

type annotation이란 개발자가 직접 명시적으로 값이 가르키는 타입을 지정하는 것이다.

**When to use?**

- 변수를 선언하고 다른 라인에서 초기화할때
- Type inference로 정확한 타입이 주어지지 않을때
- `any` 타입을 반환하는 함수
- 값을 명확하게 할 필요가 있을때

```ts
const num1: number = 5; // number type으로 type annotation
const str: string = 'Hi';

let employee : { 
    id: number; 
    name: string; 
}; 
employee = { 
  id: 100, 
  name : "John"
}
```

```ts
const json = `{"x": 10, "y": 20}`;

// type annotation을 하지 않았을때
const coord = JSON.parse(json);
coord.abc; // undefined, coord가 abc라는 프로퍼티를 가지고 있지않으므로 undefiend

// type annotation을 했을때
const coordTyped: {x:number; y:number} = JSON.parse(json);
coord.abc;  // error
```

```ts
// Type inference로 정확한 타입이 주어지지 않아 error
let numbers = [-10, -1, 12];
let numberAboveZero = false;

for(let i = 0 ; i < numbers.length; i++) {
	if(numbers[i] > 0) {
		numberAboveZero = numbers[i]; 
		// error, type inference로 정확한 타입을 알 수 없음.
	}
}

// Type annotation
let numbers = [-10, -1, 12];
let numberAboveZero: boolean | number = false;

for(let i = 0 ; i < numbers.length; i++) {
	if(numbers[i] > 0) {
		numberAboveZero = numbers[i]; 
		// error, type inference로 정확한 타입을 알 수 없음.
	}
}
```

```ts
// 비구조적(unstructured) annotation
const profile = {
	name:'alex',
	age: 20
}

const { age }: {age: number} = profile;
```

<br>

## 기타 내용들 정리

### Types

#### tuple

tuple은 배열과 유사하지만 각 엘리먼트들의 타입을 명시 할 수 있다.

```ts
let x: [string, number];
x = ["hello", 2];
```

#### enum

enum은 숫자 데이터셋에 이름을 부여하는 타입이다.

```tsx
enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;
```

#### any

알지못하는 변수 유형을 설명하는 타입이다. 컴파일시 타입검사는 하지 않는다.

#### never

절대로 발생하지 않는 값의 타입이다. 항상 예외를 발생시키거나 반환하지않는 함수 표현식이나 화살표 함수의 반환 유형에 사용된다.

```ts
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

<br>

### Type Array

type array란 각 요소가 특정한 타입의 값인 배열이다.

##### 왜 사용해야하는가?

- 다른 배열 요소들의 타입과 맞지않는(imcompatible) 값이 배열에 추가되는것을 막을 수 있다.
- `map`, `forEach`, `reduce` 함수들을 사용할때 도움을 준다.
- 배열에서 요소가 extracting 될때 type inference를 할 수 있게 된다.

```ts
const arr: string[][] = []; //2차원의 string 배열로 annotate

arr.push(100); // error
```

<br>

### Interface

interface란 객체의 프로퍼티 이름과 값 타입을 묘사하는 하나의 타입이다.

```ts
interface Vehicle {
	name: string;
	broken: boolean;
}

const oldCivic = {
	name: 'civic',
	broken:1
}

const printVehicle = (vehicle: Vehicle): void => {
	console.log(`Name : ${vehicle.name}`);
    console.log(`Broken? : ${vehicle.broken}`);
}

printVehicle(oldCivic); // error,
// Vehicle interface에서 broken을 boolean 타입으로 annotate 했으므로 
// broken이 number 타입으로 init된 oldCivic은 타입 검사에서 걸리게 됨.
```

<br>

## Literal Type and Template Literal Type

### Literal Type

리터럴 타입인 `string`과 `number ` 타입은 특정 문자열이나 숫자를 타입 지정할 수 있다.

```ts
let changingString = "Hello World";
changingString = "Ola Mundo";
// let으로 선언된 changingString은 다른 string값으로 값이 변경될 가능성이 있으므로 string으로 inference
// ^ = let changingString: string

const constantString = "Hello World";
// const로 선언된 constantString은 값이 변경될 수 없으므로 리터럴 타입으로 inference
// ^ = const constantString: "Hello World"
```

<br>

다음은 리터럴 타입과 유니언을 결합해 사용하는 예제이다.

```ts
function textAlignment(s: string, alignment: "left" | "right" | "center") {
  // ...
}
textAlignment("Hello, world", "left");
textAlignment("G'day, mate", "centre"); // *1, error

// 두번째 인자는 "left" 또는 "right" 또는 "center"로 타입 annotate 됐기 때문에 *1에서 error
```

```ts
interface Options {
  height: number;
}
function set(x: Options | "auto") {
  // ...
}
set({ height: 100 });
set("auto");
set("longer"); // error
```

<br>

#### Literal Inference

리터럴 inference란 `object`로 변수를 초기화 할때 타입 시스템에서 해당 객체의 프로퍼티가 나중에 값을 바꿀수 있다고 판단하는 것이다.

```ts
const obj = { amount: 0 };
if (someCondition) {
  obj.amount = 1;
}

// 객체 내의 amount를 number 리터럴 타입 0으로 annotate을 했지만 객체이므로 나중에 값이 바뀌어질 수 있기때문에 number로 inference됨
```

<br>

### Template Literal Type

템플릿 리터럴 타입은 `string` 리터럴 타입을 기반으로 유니언을 통해 더 많은 문자열로 확장한다.

자바스크립트 ES6의 템플릿 문자열과 같은 문법을 가지지만, type의 위치에서 사용된다.

```ts
type World = "world";

type Greeting = `hi ${World}`;
//   Greeting의 타입은 'hello world'
```

```ts
type Apple = "red" | "sweet";
type Lemon = "Yello" | "sour";

type Fruit = `I_like_${Apple | Lemon}_fruit!`;
//  Fruit의 타입은  = "red" | "sweet" | "Yello" | "sour"
```

<br>

# "?" and "!" in TypeScript

### `?` : Optional Parameters

파라미터에 `?` 접미사를 사용해 특정 타입으로 annotation 할 경우, 그 파라미터는 실제로 특정 타입과 `undefined` 타입을 가지게 된다.

옵셔널 파라미터는 사용되지 않을경우(unused, unspecified) `undefined` 값을 갖지며 값이 있을수도 있고 없을수도 있을때 사용한다.

```ts
function point(x?: number, y?: number){
    if(x) 
        console.log('X : ' + x);
    else 
        console.log('Value of X coordinate not given');
    if(y) 
        console.log('Y : ' + y);
    else 
        console.log('Value of Y coordinate not given');
}
```

```ts
// Function Call: point();
Value of X coordinate not given
Value of Y coordinate not given

// Function Call : point(10);
X : 10
Value of Y coordinate not given

// Function Call : point(10, 20);
X : 10
Y : 20
```

<br>

### `!` : Nullable Types

`!` 를 이용해 annotation된 파라미터는 타입 check시 `null`과 `undefined`를 허용한다.

```ts
const v: { p: string | null} = { p: 'text' };

console.log(v.p.toUpperCase()); // Error!

console.log(v.p!.toUpperCase()); // Fine
```

<br>

#### `??` : Nullish Coalescing Operator

`??`는 `||` 연산자와 비슷한 기능으로 `null` 또는 `undefined` 값인지 확인하는 연산자이다.

연산자의 왼쪽 변수가 `null`또는 `undfined`일 경우 연산자 우측의 값 반환한다.

```ts
const undefinedVar = undefined; 
const value = 'test'; 
const result = undefinedVar ?? value; 

// result === 'test';
```

<br>

#### `!!` 

`!!`은 자바스크립트에서 연산자로 정의된 것은 아니며 변수를 `boolean` 타입으로 형변환하는 역할을 한다.

값이 할당되어 있으면 `true`를 반환, `0`, `null`, `undefined`의 값을 가질 경우 `false`를 반환한다.

```
const array = [1,2];
!!array[0]; // true
!!array[1]; // true
!!array[2]; // false, undefined

const obj = {
  value: 5,
  node: null
};
!!obj.value; // true
!!obj.node;  // false, null

let v = 0;
!!v // false, 0
```

<br>

<br>

------

##### Reference

* [TypeScript](https://www.typescriptlang.org/)
* [JS Double Bang — or “The Not Operator Part !!”](https://medium.com/@edplatomail/js-double-bang-or-the-not-operator-part-40e55d089bf0)
* Udemy: React and Typescript: Build a Portfolio Project(by Stephen Grider)

