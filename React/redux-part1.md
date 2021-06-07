# Redux (part 1)

Redux는 "action"이라는 이벤트를 사용해 어플리케이션의 state를 관리하고 업데이트하기 위한 라이브러리 및 패턴이다. Redux는 state들의 중앙 저장소(centralized store)의 역할을 하며 state가 예측 가능한 방식(predictable)으로만 업데이트 되도록 하는 규칙을 제공한다.

Redux에서 제공되는 패턴들과 도구들을 사용하면 언제/어디서/왜/어떻게 state가 업데이트되고, state가 업데이트 될 때 어플리케이션의 로직이 어떻게 작동하는지 쉽게 이해할 수 있다.

<br>

## Background Concepts

Redux를 사용하기 위해선 몇 가지 개념과 용어에 대해 알아야한다.

<br>

### State Management

```javascript
function Counter() {
  // State: a counter value
  const [counter, setCounter] = useState(0)

  // Action: code that causes an update to the state when something happens
  const increment = () => {
    setCounter(prevCounter => prevCounter + 1)
  }

  // View: the UI definition
  return (
    <div>
      Value: {counter} <button onClick={increment}>Increment</button>
    </div>
  )
}
```

위 예제는 버튼을 클릭하면 counter state가 증가하는  `Counter` 컴포넌트이다. 이 컴포넌트는 세 가지 부분으로 구성된 독립형 어플리케이션(self-contained app)이다.

- state : 어플리케이션 구동의 바탕이 되는 상태(the source of truth that drives our app)
- view : UI 정의
- actions : 사용자 입력을 바탕으로 발생하는 이벤트이며 state가 업데이트 되는 트리거

이 어플리케이션은 state에 따라 view가 렌더링되며 action에 의해 state가 업데이트되면 다시 view가 렌더링되는 흐름인 단방향 데이터 플로우(one-way data flow)의 예이다.

단방향 데이터 플로우는 동일한 state를 공유하고 사용하는 다수의 컴포넌트가 있을 경우 비효율적이다. 이를 해결하기 위한 방법은 컴포넌트에서 공유되는 state를 추출해 컴포넌트 트리 외부 중앙에 위치시켜서 컴포넌트가 트리의 어느 부분에 위치하든 state를 액세스할 수 있게 하는 것이다.

이것이 Redux의 기본 아이디어이다.

<br>

### Immutability

Redux에서 state의 업데이트시, 불변성을 유지해야하는 원칙이 있다. 따라서 기존의 state를 직접 수정하는 것이 아닌 기존 state를 포함한 새로운 state를 생성하는 방식으로 state를 업데이트한다.

<br>

<br>

## Redux Terminology

### Actions

action은 `type` 필드를 가지고 있는 객체이며 어플리케이션에서 발생한 어떤 동작을 설명하는 이벤트이다.

action은 발생한 동작에 대한 추가적인 정보를 담고 있는 `payload` 필드를 가질 수 있다.

```javascript
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```

<br>

### Reducers

reducer는 현재의 state와 action을 전달 받으며 state를 어떻게 업데이트 할 것인가를 결정하고 새로운 state를 반환한다. `(state,action) => newState`

전달받은 action(이벤트) type을 기반으로 이벤트를 처리하는 이벤트 리스너 개념으로 생각하면 된다.

reducer는 항상 아래와 같은 규칙을 지켜야한다.

- `state`와 `action` 인자를 기반으로 새로운 state 값 만 계산해야한다.
-  기존 state를 수정할 수 없다. 기존 state를 복사하고 복사된 값을 변경해 불변성을 지켜야한다.
- 비동기 로직, 랜덤 값 계산 등 side effect를 발생시켜선 안된다.

<br>

reducer는 처리할 action인지 확인 후, 처리할 action이 맞을 경우 state의 복사본을 만들어 새로운 값으로 복사본을 업데이트하고 반환한다. 그렇지 않을 경우에는 기존 state를 반환한다.

```javascript
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/incremented') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

<br>

### Store

store는 reducer를 전달받아 생성되며 현재 state 값을 반환하는 `getState` 메소드가 있다.

```javascript
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

<br>

### Dispatch

Redux store는 `dispatch`라는 메소드를 가지고 있다. state를 업데이트하는 유일한 방법은  `store.dispatch()`를 호출하고 action 객체를 전달하는 것이다. store는 reducer 함수를 실행해 새로운 state 값을 내부에 저장한다. 또한 `getState()`를 통해 업데이트된 값을 확인할 수 있다.

```javascript
store.dispatch({ type: 'counter/incremented' })

console.log(store.getState())
// {value: 1}
```

dispatch는 action을 발생시키는 이벤트 트리거 개념으로 생각하면 된다.

<br>

### Action Creator

action creator는 action을 발생시키는 함수이다. `store.dispatch({ type: '...'})`을 통해 action을 발생시킬 수 있지만 동일한 작업이 반복될 경우 비효율적이므로 action creator를 사용해 store에 action을 보낼 수 있다.

```javascript
const doAddToDoItem = payload => ({ type: 'TODO_ADDED', payload })

store.dispatch(doAddToDoItem('be awesome today'))
```

<br>

 `store.dispatch()` 부분을 수행하는 새로운 "wrapper" 함수를 생성함으로써 dispatch 과정을 손쉽게 할 수 있다.

```javascript
// assume we've created a store here
const store = createStore(someReducer)

// and we have our plain action creator function from above
const doAddToDoItem = payload => ({ type: 'TODO_ADDED', payload })

// we'd simply need to make a new function that did both things:
const boundActionCreator = text => store.dispatch(doAddToDoItem(text))
```

라이브러리에 내장된 `bindActionCreators()` 함수를 사용해 위 "wrapper" 함수를 대체할 수 있다. `bindActionCreators()` 함수는 action creator를 가져와 자동으로 바인딩 해주기 때문에 action의 수가 많을때 유용하다.

<br>

### Selector

selelctor는 store state 값에서 특정 부분의 정보를 추출하는 함수이다. 

```javascript
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

<br>

## Redux Application Data Flow

![dataflow](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

<br>

- #### Redux cycle

  - Action Creator → Action → Dispatch → Reducer → State

<br>

- #### 초기설정

  - root reducer 함수를 사용해 Redux store 생성
  - store는 root reducer를 한번 호출하고 반환 값을 초기 state로 저장 
  - UI가 처음 렌더링 될 때, UI 컴포넌트는 Redux store의 현재 state에 액세스하고 렌더링 하기 위한 데이터 결정


- #### 업데이트

  - 버튼 클릭 등의 이벤트 발생
  - store에 `dispatch({type: 'counter/incremented'})` 작업 전달
  - store는 이전 `state`와 현재 `action`으로 reducer 함수를 다시 실행하고 반환 값을 새로운 `state`로 저장
  - store의 데이터가 필요한 UI 컴포넌트들은 필요한 state가 변경되었는지 확인 
  - 새로운 데이터로 리렌더링된 UI 컴포넌트들이 화면에 출력

<br>

<br>

------

**Reference**

- [Redux Fundamentals, Part 2: Concepts and Data Flow](https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow)

- [Action Creators](https://read.reduxbook.com/markdown/part1/04-action-creators.html)