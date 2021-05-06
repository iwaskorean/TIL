# Expensive Operation and Big O Notation

> *Knowing Big O helps and facilitates developers being aware of the efficiency of an algorithm so they can create applications with good performance.*

빅오 표기법은 입력에 따른 알고리즘의 실행 시간 즉, 알고리즘의 효율성에 대한 수학적 표현이다. 일반적으로 최악의 경우를 나타낼때 사용된다.

<br>

### Constant-Time Algorithm

#### **O(1) - *"Order 1"***

복잡도(항목 수)에 관계없이 시간(반복)이 일정할때 상수 시간(constant time)을 갖는다고 하며 이 경우에 알고리즘이 문제를 해결하는데 오직 한 단계(1회 반복)만 거친다.

```javascript
const getLast = items =>
  items[items.length-1];

// example case
getLast(['a', 'b', 'c', 'd']); // d (1회 반복)

getLast(['a', 'b', 'c', 'd', 'e', 'f', 'g']);// g(1회 반복)
```

<br>

### Linear-Time Alogrithm

#### O(N) - *"Order N"*

항목 수와 반복 수가 같을 때 즉, N개의 요소에 대해 N개의 반복이 필요한 경우 선형 시간(linear time)을 갖는다고 한다.

```javascript
const findIndex = (items, match) => {
  for (let i = 0, total = items.length; i < total; i++)
    if (items[i] == match)
      return i;
   return -1;
};

// example case
const array= ['a', 'b', 'c', 'd'];

findIndex(array, 'a'); // 0  (1회 반복 - best case)
findIndex(array, 'd'); // 3  (4회 반복 - worst case, 4개의 요소에 대해 4회 반복)
findIndex(array, 'e'); // -1 (4회 반복 - worst case)
```

<br>

### Quadratic-Time Alogrithm

#### O(N^2) - *"Order N squared"*

최악의 시간(반복)이 입력 수의 제곱일때 이차 시간(quadratic time)을 가진다고하며 시간(반복)은 입력 수와 관련하여 기하급수적으로 늘어난다.

```javascript
const buildSquareMatrix = items => {
  let matrix = [];
  for (let i = 0, total = items.length; i < total; i++){ 
    matrix[i] = [];
    for (let j = 0, total = items.length; j < total; j++)
      matrix[i].push(items[j]);
  }
  return matrix;
};

// example case
buildSquareMatrix(['a', 'b', 'c']); 
/* 3개의 요소에 대해 9회의 반복
returns:
[
  ['a', 'b', 'c'],
  ['a', 'b', 'c'],
  ['a', 'b', 'c']
]
/*
```

<br>

### Logarithmic-Time Algorithm

#### O(log n | n log n) - *"Order log N | N log N"*

입력 크기에 따라 처리 시간이 증가하는 검색 / 정렬 알고리즘에서 많이 사용하며 큰 컬렉션들을 처리할때 효과적이다. 구성 요소를 하나씩 확인하는 대신 반으로 분할하고 확인한 후 다시 반으로 분할하는 것을 반복하는 방법이다.

Quicksort, Merge-Sort 알고리즘에서 사용된다.

```javascript
const quickSort = list => {
  if (list.length < 2) 
    return list;
  let pivot = list[0];
  let left  = []; 
  let right = [];
  for (let i = 1, total = list.length; i < total; i++){
    if (list[i] < pivot)
      left.push(list[i]);
    else
      right.push(list[i]);
  }
  return [
    ...quickSort(left), 
    pivot, 
    ...quickSort(right)
  ];
};

// example case
quickSort( ['q','a','z','w','s','x','e','d','c','r']);
// ["a", "c", "d", "e", "q", "r", "s", "w", "x", "z"]
```

<br>

### Exponential-Time Algorithm

#### O(2^n) - *"Order 2^N*

입력에 따라 처리하는 시간이 제곱에 비례할때 그 알고리즘은 지수 시간을 가진다고 말할 수 있다. 가능한 모든 조합과 방법을 시도할때 사용된다.

<br>

### Big-O Complexity

![graph](https://cdn-media-1.freecodecamp.org/images/1*KfZYFUT2OKfjekJlCeYvuQ.jpeg)

![rank](https://cdn-media-1.freecodecamp.org/images/1*NHVggTVMGjGOe7SxtSgIpQ.png)

<br>

------

**Reference**

- [Big O Notation in JavaScript](https://medium.com/cesars-tech-insights/big-o-notation-javascript-25c79f50b19b)
- [Time complexity](https://en.wikipedia.org/wiki/Time_complexity#Logarithmic_time)
- [What is Big O Notation Explained: Space and Time Complexity](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/)
