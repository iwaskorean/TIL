# Regular Expression

> 자바스크립트에서 정규 표현식은 객체이며 문자열에서의 문자 조합과 매칭시키기 위한 패턴이다.

<br>

### Creating a regular expression

- 정규 표현식 리터럴 

  ```
  let re = /ab+c/;
  ```

  - `/`로 감싸는 패턴

  - 스크립트가 로드될때 컴파일 된다.

  - 정규 표현식이 상수(constant)라면 리터럴로 사용하는것이 성능을 향상시킬 수 있다.

- 생성자 함수 호출

  ```
  let re = new RegExp('ab+c');
  ```

  - 실행 시점에 컴파일 된다.

  - 정규 표현식이 변경될 가능성이 있는 경우나 사용자의 입력과 같이 다른 곳에서 문자열을 가져오는 경우 등에 사용된다.

<br>

### Writing a regular expression pattern

- 단순한 패턴

  - 문자열에 직접적으로 대응되는 문자들을 찾을 때 사용된다.

  - 예를 들어, `/abc/`는 `abc`라는 문자가 순서대로 있는 문자 조합과 대응된다.

- 특수 문자

  - 하나 이상의 `b`를 찾거나 공백을 찾는 등의 직접적인 매칭 이상의 문자 조합을 찾을때 사용된다.

  - 예를 들어, `/ab*c/`는 `a` 다음에 0개 이상의 `b`가 존재하고 `c`가 오는 문자 조합과 대응 된다.

<br>

### Using special characters

- #### Assertions

  - 어설션은 문장이나 단어의 시작과 끝 문자에 대한 대응이나 조건부 표현을 위한 패턴이다.

  - ##### Types

    | 문자   | 의미                              |
    | ------ | --------------------------------- |
    | ^      | 입력의 시작과 대응                |
    | $      | 입력의 끝과 대응                  |
    | \b     | 단어 경계와 대응                  |
    | x(?=y) | x 다음에 y가 오는 경우의 x와 대응 |
    | x(?!y) | x 뒤에 y가 없는 경우의 x와 대응   |

  - ##### Example

    ```
    // ^, $ : 'A'로 시작하는 과일, 'e'로 끝나는 과일 찾기
    let fruits = ['Apple', 'Watermelon', 'Orange', 'Avocado', 'Strawberry'];
    
    fruits.filter((fruit) => /^A/.test(fruit)); // [ 'Apple', 'Avocado' ]
    fruits.filter((fruit) => /e$/.test(fruit)); // ['Apple', 'Orange']
    
    // \b : 끝이 'en', 'ed'로 되어있는 단어를 포함하는 문자열 찾기
    let fruitsWithDescription = ["Red apple", "Orange orange", "Green Avocado"];
    
    fruitsWithDescription.filter((descr) => /(en|ed)\b/.test(descr)) // ['Red apple', 'Green Avocado']
    
    fruits.filter((fruit) => /^A+\w+e$/.test(fruit)) // ['Apple']
    
    // 특정 문자 앞에 오는 단어 찾기
    const text = 'A big tiger'
    const regExp = /\w+(?= tiger)/;
    text.match(regExp); // big
    ```

    <br>

- #### Character classes

  - 문자와 숫자를 구분하는 등의 문자 종류를 구분하기 위한 패턴

  - ##### Types

    | 문자 | 의미                                      |
    | ---- | ----------------------------------------- |
    | .    | 줄바꿈 문자를 제외한 단일 문자와 대응     |
    | \d   | 숫자와 대응                               |
    | \D   | 숫자가 아닌 문자와 대응                   |
    | \n   | 줄 바꿈과 대응                            |
    | \0   | null 문자와 대응                          |
    | \s   | 공백과 대응, `/S`는 공백 아닌 것과 대응   |
    | \w   | 문자와 대응, `/W`는 문자가 아닌 것과 대응 |

  - ##### Example

    ```
    // 문자+숫자로 이루어진 단어 찾기
    const text = 'I have iphone10, ipad8 and macbook';
    const regExp = /\w+\d+/g;
    
    text.match(regExp); // ["iphone10", "ipad8"]
    ```
    
    <br>

- #### Group and ranges

  - 문자의 그룹 및 범위를 나타내기 위한 패턴

  - ##### Types

    | 문자             | 의미                                  |
    | ---------------- | ------------------------------------- |
    | x\|y             | x 또는 y와 대응                       |
    | [xyz], [x-z]     | 포함된 문자 중 하나와 대응            |
    | [^xyz] , [ ^x-z] | 해당 패턴에 포함되지 않은 항목과 대응 |

  - ##### Example

    ```
    // 모음으로 시작하는 두 문자 이상으로 이루어진 단어 찾기
    const text = 'There was a long silence after this.';
    const regExp = /\b[aeiouy]\w+/g;
    
    text.match(regExp) // ["after"]
    ```
    
    <br>

- #### Quantifiers

  - 대응되는 문자 또는 표현식의 수를 나타내기 위한 패턴

  - ##### Types

    | 문자  | 의미                                                         |
    | ----- | ------------------------------------------------------------ |
    | *     | * 앞의 표현식이 0회 이상  반복되는 부분과 대응. {0,}과 같은 의미 |
    | +     | + 앞의 표현식이 1회 이상 반복되는 부분과 대응. {1,}과 같은 의미 |
    | ?     | ? 앞의 표현식이 0 또는 한번 나오는 부분과 대응               |
    | {x, } | 최소한 x회 이상 대응, {x, y}는 최소 x회 최대 y회 대응        |

  - ##### Example

    ```
    const text1 = 'He asked his neighbour a favour.';
    const text2 = 'He asked his neighbor a favor.';
    
    const regExp = /\w+ou?r/g;
    
    text1.match(regExp); // ['neighbour', 'favour']
    text2.match(regExp); // ['neighbor', 'favor']
    ```
    
    <br>

- #### Flag

  - g : 전역 검색, 문자열 전체에 대한 패턴 검색

  - i : 대소문자 구분하지 않음

  - m : 개행문자 허용, 문자열의 행이 바뀌어도 패턴 검색

<br>

### Using a regular expression in JS

- `exec`() : 대응되는 문자열을 찾고 그에 대한 정보를 가지고 있는 배열을 반환한다.

  ```
  let regex1 = RegExp('foo*', 'g');
  let str1 = 'table football, foosball';
  let array1;
  
  while ((array1 = regex1.exec(str1)) !== null) {
    console.log(`Found ${array1[0]}. Next starts at ${regex1.lastIndex}.`);
    // expected output: "Found foo. Next starts at 9."
    // expected output: "Found foo. Next starts at 19."
  }
  ```

- `test()` : 대응되는 문자열이 있을경우 `true`, 그렇지 않은경우 `false`를 반환한다.

  ```
  let str = 'table football';
  
  let regExp = new RegExp('foo*');
  
  console.log(regExp.test(str)); // true
  ```

- `match()` : 대응되는 문자열을 찾아 그에 대한 정보를 가지고 있는 배열을 반환한다.

  - 캡처 그룹을 알고 싶고 전역플래그가 설정되어 있는경우 `RegExp.exec()`를 대신 사용해야한다.

  ```
  let str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
  let regexp = /[A-E]/gi;
  
  str.match(regexp);
  // ['A', 'B', 'C', 'D', 'E', 'a', 'b', 'c', 'd', 'e']
  ```

- `search()` : 대응되는 문자열이 있는 인덱스를 반환한다. 찾지 못할 경우 -1을 반환한다.

- `replace()` : 대응되는 문자열을 다른 문자열로 변경한다.

  ```
  let str = 'Twas the night before Xmas...';
  
  str.replace(/xmas/i, 'Christmas');
  // "Twas the night before Christmas..."
  ```

  

<br>

### Examples

- 도메인 이름

  ```
  ^[a-zA-Z0-9][a-zA-Z0-9]{1,61}[a-zA-Z0-9]\.[a-zA-Z]{2,}$
  ```

- 이메일 주소

  ```
  ^([0-9a-zA-Z_-]+)@([0-9a-zA-Z_-]+)(\.[0-9a-zA-Z_-]+){1,2}$/
  ```

- 핸드폰 번호

  ```
  ^(010|019|011)-\d{4}-\d{4}
  ```

- Twitter 사용자 이름

  ```
  ^[a-zA-Z0-9_\-.]{3,15}$
  ```

- 내용없는 빈 줄

  ```
  \n\s*\r
  ```

- 날짜 - 4자리 연도, 2자리 월/일

  ```
  ^\d{4}-\d{1,2}-\d{1,2}$
  ```

<br>

<br>

------

**Reference**

- [Regular expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)