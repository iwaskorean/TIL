# Type Challenges

[Type Challenges](https://github.com/type-challenges/type-challenges)를 풀어보며 정리한 내용이다.

## Mapped Type

mapped type은 기존에 정의된 타입을 바탕으로 새로운 타입을 생성한다.

mapped type 작성을 위한 주요 키워드는 다음과 같다.

- **keyof**: 객체의 키를 리터럴 값으로 추출한다.
- **대괄호를 통한 프로퍼티 접근**(`[P in keyof T]: T[P]`): 대괄호와 keyof 연산자를 사용하면 프로퍼티들을 추출한 다음 반복할 수 있다.
- **조건부 타입**: `T extends U ? X : Y`는 T가 U로 확장가능 한 경우 X, 그렇지 않은 경우 Y로 annotation된다.
- **infer**: 명시적으로 타입을 추론할 수 있으며 조건부 타입에서 타입 파라미터를 참조할 수 있다.
- **never**: 발생하지 않는 값의 타입 즉, "어떠한 타입도 아니다" 라는 의미며 어떤 값도 할당할 수 없는 타입이다. 잘 못된 타입이 전달되는 것을 방지할 수 있다.

```ts
{ [ P in K ] : T }
{ [ P in K ] ? : T }
{ readonly [ P in K ] : T }
{ readonly [ P in K ] ? : T }
```

```ts
type Sizes = 'large' | 'medium' | 'small';
type Size = { [K in Sizes]: number };
const size: SizeInfo = {
  large: '100cm',
  medium: '80cm',
  small: '60c,',
};
```

## Pick(4-Pick)

- Pick은 유틸리티 타입으로 Pick<T, K>는 T에서 프로퍼티 K의 집합을 선택해 타입을 구성한다.
- 내장 제네릭 Pick을 직접 구현해보는 문제다.

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyPick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
};

// solution

type MyPick<T, K extends keyof T> = {
  [key in K]: T[key];
};
```

## 11-Tuple to Object

- 튜플을 받아 원소의 값을 key/value로 갖는 객체 타입을 반환하는 타입을 구하는 문제다.

```ts
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;

type result = TupleToObject<typeof tuple>;
// expected { tesla: 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}

// solution
type TupleToObject<T extends readonly string[]> = {
  [key in T[number]]: key;
};
```

## Exclude(43-Exclude)

- Exclude는 유틸리티 타입으로 Exclude<T, U>는 T 중 U로 확장될 수 있는 타입을 제외하는 타입을 반환한다.
- 내장 유틸리티 타입 Exclude를 직접 구현하는 문제다.

```ts
let test: MyExclude<string | boolean | number, string>;
// let test: boolean | number;

// solution
type MyExclude<T, U> = T extends U ? never : T;
```

## Omit(3 - Omit)

- Omit은 유틸리티 타입으로 Omit<T, K>은 T에서 K 프로퍼티를 제거해 새로운 객체 타입을 생성한다.
- 내장 유틸리티 타입 Omit을 직접 구현하는 문제다.

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyOmit<Todo, 'description' | 'title'>;

const todo: TodoPreview = {
  completed: false,
};

// solution
type MyOmitM<T, K extends keyof T> = {
  [key in Exclude<keyof T, K>]: T[key];
};
// Exclude<keyof Todo, 'description' | 'title'> === 'completed'
```

....
