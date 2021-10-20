# Testing

테스트는 어플리케이션이 제대로 동작하는지 검증하기 위한 작업이다. 사용자의 유즈케이스가 테스트로 커버되어있다면 어플리케이션의 일부분이 제대로 동작하지 않을 때 빠르게 피드백을 얻을 수 있다.

프로젝트의 모든 기능을 하나하나 직접 테스트하는 것은 매우 비효율적이다. 그렇기 때문에 테스트 코드를 작성해 테스트 시스템이 자동으로 테스트하는 테스트 자동화가 필요하다.

테스트는 유닛 테스트(unit test)와 통합 테스트(integrated test), E2E 테스트(end to end test)로 나누어진다.

<br>

### Unit Test

유닛 테스트란 아주 조그마한 단위 즉, 완전히 분리된 컴포넌트들을 대상으로하는 테스트를 의미한다. 유닛 테스트의 예시는 다음과 같다.

- 컴포넌트가 정상적으로 렌더링되는가 확인한다.
- 컴포넌트의 특정 함수를 실행하면 state가 의도한 대로 바뀌는지 확인한다.

유닛 테스트를 통해 컴포넌트의 기능이 잘 작동하는지 확인할 수 있지만 기능들이 전체적으로 잘 동작하는지는 확인하기 어렵다는 단점이 있다.

<br>

### Integrated Test

전체적으로 기능들이 정상 작동하는가를 확인하기 위해서는 통합 테스트가 필요하다. 통합 테스트의 예시는 다음과 같다.

- 다수의 컴포넌트들을 렌더링하고 서로 상호작용이 정상적으로 동작하는지 확인한다.
- DOM 이벤트를 발생시켰을 때, 의도한 UI의 변화가 발생하는지 확인한다.

유닛 테스트는 하나의 작은 부분에 초점을 둔다면, 통합 테스트는 여러 요소들을 고려해 작성한다.

<br>

### End to End Test

E2E 테스트는 어플리케이션이 의도한대로 작동하는지 확인하기 위해 처음부터 끝까지의 전체 기능을 테스팅하는 테스트이다. E2E 테스트의 예시는 다음과 같다.

- 전체적인 유저 플로우를 확인한다.
- 테스트 커버리지의 증가를 확인한다.
- 하위 시스템의 문제점을 식별한다.

cypress와 같은 프레임워크를 통해 테스트할 수 있다.

<br>

### Testing Library

테스팅 라이브러리는 테스트를 사용자 관점에서 접근한다. 그렇기 때문에 최종적으로 통합 테스트로 연결된다.

테스트 자동화에 가장 많이 사용되는 라이브러리는 jest와 @testing-library/react의 조합이다. jest는 mocha나 jasmine으로 대체할 수 있으며 @testing-library/react는 enzyme로 대체할 수 있다.

<br>

## React Testing Library

testing library는 모든 테스트를 DOM 위주로 진행한다. 또한 enzyme에 비해 필수 기능만 제공하므로 가벼우며 일관성 있고 좋은 관습을 따르는 테스트 코드를 작성할 수 있도록 해준다.

<br>

### Snapshot Testing

스냅샷 테스팅이란 이전에 저장된 렌더링 결과와 테스트 시점에 생성되는 새로운 렌더링 결과가 일치하는지 확인하는 테스트이다.

테스트 대상이 되는 기능의 구현이 변경되어 실제 결과와 이전 스냅샷이 다를 경우 테스트 케이스가 실패하게 되는데, 이 경우 다시 새로운 스냅샷을 만들어 기존의 스냅샷을 교체하는 방식으로 스냅샷도 유지보수를 한다.

스냅샷 테스팅은 함수의 예상 결과를 직접 코디할 필요가 없어 편리하지만 무분별하게 사용할 경우 유지보수가 어렵다는 단점이 있다.

위에서 언급한 jest를 사용하면 스냅샷 테스팅을 보다 쉽게 수행할 수 있다.

<br>

### Methods and Queries

테스팅시 사용되는 메소드는 다음과 같다.

- `render()` : `@testing-library/react` 모듈에 선언되어 있으며 인자로 들어온 컴포너트로 DOM을 생성한다.
- `expect()` : `jest` 모듈에 선언되어 있으며 테스트 결과와 예상 값이 일치하는지 확인한다.
- `toMatchSnapshot()` : `jest` 모듈에 선언되어 있으며 파일 스냅샷 테스팅을 지원하기 위한 함수이다. 생성되는 스냅샷을 테스트 파일이 아닌 테스트 파일이 위치하는 경로에  `__snapshots__` 디렉토리가 생성되고 디렉토리 내부에 `snap` 파일이 생성된다.

<br>

렌더링된 DOM 트리에 접근하기 위한 방법은 testing library에서 export된 screen 객체를 이용하는 것이다.

screen 객체는 다양한 쿼리를 제공하며, 쿼리들은 DOM에 접근할 수 있는 함수들이다.

- `getBy*` : 동기적이며, 해당하는 요소가 현재 DOM 안에 존재하는가 확인한다. 그렇지 않을 경우 에러를 발생시킨다.(`getByTestId`, `getByText`, `getByRole`)

- `findBy*` : 비동기적이며, 해당 요소를 찾을 때까지 일정 시간을 기다린다. 일정 시간이 지난후에도 요소를 찾지 못할 경우 에러를 발생시킨다.(`findByText`)

- `queryBy*` : `getBy*`와 마찬가지로 동기적이지만 해당 요소를 찾지 못하더라도 에러가 아닌 null 값을 리턴한다.

- `ByText` : 요소가 가지고 있는 텍스트 값으로 DOM을 선택한다.

- `ByAltText` : `img`와 같이 `alt` 속성을 가지고 있는 요소를 선택한다.

- `ByLableText` : `input` 요소의 `label`의 내용으로 요소를 선택한다.

- `ByDisplayValue` : `input`, `textarea`, `select` 요소의 `value` 프로퍼티로 요소를 선택한다.

- `ByTestId` : 특정 DOM에 직접 테스트할 때 사용할 `id`를 통해 선택한다.

  ```javascript
  <div data-testid="testId">Example</div>
  const Example = getByTestId('testId');
  ```

- `ByRole` : 특정 `role` 값을 포함하고 있는 요소를 선택한다.

[testing library 공식 문서](https://testing-library.com/docs/queries/about/#priority)에서는 다음과 같은 우선순위를 권장한다.

1. 일반적인 쿼리
   1. `getByRole`
   2. `getByLabelText`
   3. `getByPlaceholderText`
   4. `getByText`
   5. `getByDisplayValue`
2. 시맨틱 쿼리
   1. `getByAltText`
   2. `getByTitle`
3. 테스트 id
   1. `getByTestId`

`screen` 객체와 쿼리를 사용한 예시 테스트 코드는 다음과 같다.

```javascript
import {render, screen} from '@testing-library/react'

render(
  <div>
    <label htmlFor="example">Example</label>
    <input id="example" />
    <div>Hello World</div>
  </div>,
)

const exampleInput = screen.getByLabelText('Example');
const textMatch = screen.getByText('Hello World');
```

<br>

### Event Handling

어플리케이션을 개발할 때 사용자가 발생시키는 이벤트가 예상대로 반응하는지에 대한 테스트도 필요하다.

다음은 어플리케이션과 사용자의 인터랙션을 확인하기 위한 이벤트 테스팅 테스트 코드 작성 방법이다.

`@testing-library/react` 모듈에서 제공하는 `fireEvent` 메소드를 사용하면 쉽게 사용자 이벤트를 발생시킬 수 있다.

```javascript
fireEvent.[eventName](HTMLElement, eventProperties: Object);

fireEvent.change(email, { target: { value: "user@test.com" } });
```

`fireEvent`는 실제로 발생해야하는 모든 사용자 이벤트를 발생시키기 어렵다는 단점이 있다. 이 경우 `@testing-library/user-event`에서 제공되는 `userEvent` 메소드를 사용하면 브라우저와 사용자의 인터랙션에 대해 더 복잡한 시뮬레이션이 가능하다.

```javascript
userEvent.[eventName](HTMLElement, eventProperties)

userEvent.type(email, "user@test.com");
```

`userEvent`를 사용해 `type`, `click`,  키를 누르는 것을 시물레이션하는 `keyboard`, `upload`, `clear`, `selectOption` 등의 이벤트를 테스트할 수 있다. ([공식 문서 참고](https://testing-library.com/docs/ecosystem-user-event/))

<br>

###### ...

###### Todo : form testing, counter example, mocking and test for asynchronous

<br>

<br>

------

**Reference**

- [Testing Library](https://testing-library.com/docs/)
- [[번역] 초보자를 위한 React 어플리케이션 테스트 심층 가이드](https://blog.rhostem.com/posts/2020-10-14-beginners-guide-to-testing-react-1#overview_of_testing_react_apps)
- [벨로퍼트와 함께하는 리액트 테스팅](https://velog.io/@velopert/series/react-testing)
- [Jest로 스냅샷(snapshot) 테스트하기](https://www.daleseo.com/jest-snapshot/)
- [유저 이벤트 테스트 (@testing-library/user-event)](https://www.daleseo.com/testing-library-user-agent/)
