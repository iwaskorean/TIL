# Bubbling and Capturing



### 1. Bubbling

- 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작, 가장 최상단의 부모 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작

  ```
  // 3개의 요소가 FORM > DIV > P 형태로 중첩된 구조
  
  <style>
    body * {
      margin: 10px;
      border: 1px solid blue;
    }
  </style>
  
  <form onclick="alert('form')">FORM
    <div onclick="alert('div')">DIV
      <p onclick="alert('p')">P</p>
    </div>
  </form>
  ```

- 가장 하위에 배치된 `<p>`요소를 클릭하면 `p` → `div` → `form` 순서로 3개의 alert 동작

  1. `<p>`에 할당된 `onclick` 핸들러 동작
  2. 바깥의 `<div>`에 할당된 핸들러 동작
  3. 그 바깥의 `<form>`에 할당된 핸들러 동작
  4. `document` 객체를 만날 때까지, 각 요소에 할당된 `onclick` 핸들러 동작

- `event.target`

  -  이벤트가 발생한 가장 안쪽의 요소는 target이라 불리고, event.target을 통해 접근

- 버블링 중단

  - `event.stopPropagation()` 메소드를 통해 이벤트 핸들러를 동작하지 않게 할 수 있음
  - 버블링을 중단할때에는 structure 와 전체적인 흐름을 생각해야함
  - 꼭 필요한 특수한 상황을 제외하고 사용 지양



### 2. Capturing

- 표준 DOM 이벤트에서의 3가지 단계의 흐름

  1. 캡처링 : 이벤트가 하위 요소로 전파되는 단계
  2. 타깃 : 이벤트가 실제 타깃 요소에 전달되는 단계
  3. 버블링 : 이벤트가 상위 요소로 전파되는 단계

  ![3phaseOfDomEvents](https://javascript.info/article/bubbling-and-capturing/eventflow.svg)

  - `<td>` 를 클릭하면 최상위 부모에서 시작해 아래로 전파되는 **캡처링(1)**, 이벤트가 타깃 요소에 도착해 실행되는 **타깃(2)**, 다시 최상위 부모로 전파되는 **버블링(3)** 단계를 통해 요소에 할당된 이벤트 핸들러가 호출
  - 3개의 이벤트 흐름에서 이벤트가 실제 타깃요소에 전달되는 **타깃(2)**단계는 별도로 처리 되지 않고, **캡처링(1)**과 **버블링(2)**은 타깃 단계에서 트리거됨

- 캡처링 단계에서 어떠한 작업을 처리하는 경우는 흔치 않음





------

**Reference**

- [Bubbling and Capturing](https://javascript.info/bubbling-and-capturing)