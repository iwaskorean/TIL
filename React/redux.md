# Redux (part 1)

Redux는 "action"이라는 이벤트를 사용해 어플리케이션의 state를 관리하고 업데이트하기 위한 라이브러리 및 패턴이다. Redux는 state들의 중앙 저장소(centralized store)의 역할을 하며 state가 예측 가능한 방식(predictable)으로만 업데이트 되도록 하는 규칙을 제공한다.

Redux에서 제공되는 패턴들과 도구들을 사용하면 언제/어디서/왜/어떻게 state가 업데이트되고, state가 업데이트 될 때 어플리케이션의 로직이 어떻게 작동하는지 쉽게 이해할 수 있다.

## Background Concepts

Redux를 사용하기 위해선 몇 가지 개념과 용어에 대해 알아야한다.

### State Management

```javascript
function Counter() {
  // State: a counter value
  const [counter, setCounter] = useState(0);

  // Action: code that causes an update to the state when something happens
  const increment = () => {
    setCounter((prevCounter) => prevCounter + 1);
  };

  // View: the UI definition
  return (
    <div>
      Value: {counter} <button onClick={increment}>Increment</button>
    </div>
  );
}
```

위 예제는 버튼을 클릭하면 counter state가 증가하는 `Counter` 컴포넌트이다. 이 컴포넌트는 세 가지 부분으로 구성된 독립형 어플리케이션(self-contained app)이다.

- state : 어플리케이션 구동의 바탕이 되는 상태(the source of truth that drives our app)
- view : UI 정의
- actions : 사용자 입력을 바탕으로 발생하는 이벤트이며 state가 업데이트 되는 트리거

이 어플리케이션은 state에 따라 view가 렌더링되며 action에 의해 state가 업데이트되면 다시 view가 렌더링되는 흐름인 단방향 데이터 플로우(one-way data flow)의 예이다.

단방향 데이터 플로우는 동일한 state를 공유하고 사용하는 다수의 컴포넌트가 있을 경우 비효율적이다. 이를 해결하기 위한 방법은 컴포넌트에서 공유되는 state를 추출해 컴포넌트 트리 외부 중앙에 위치시켜서 컴포넌트가 트리의 어느 부분에 위치하든 state를 액세스할 수 있게 하는 것이다.

이것이 Redux의 기본 아이디어이다.

### Immutability

Redux에서 state의 업데이트시, 불변성을 유지해야하는 원칙이 있다. 따라서 기존의 state를 직접 수정하는 것이 아닌 기존 state를 포함한 새로운 state를 생성하는 방식으로 state를 업데이트한다.

## Redux Terminology

### Actions

action은 `type` 필드를 가지고 있는 객체이며 어플리케이션에서 발생한 어떤 동작을 설명하는 이벤트이다.

action은 발생한 동작에 대한 추가적인 정보를 담고 있는 `payload` 필드를 가질 수 있다.

```javascript
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk',
};
```

### Reducers

reducer는 현재의 state와 action을 전달 받으며 state를 어떻게 업데이트 할 것인가를 결정하고 새로운 state를 반환한다. `(state,action) => newState`

전달받은 action(이벤트) type을 기반으로 이벤트를 처리하는 이벤트 리스너 개념으로 생각하면 된다.

reducer는 항상 아래와 같은 규칙을 지켜야한다.

- `state`와 `action` 인자를 기반으로 새로운 state 값 만 계산해야한다.
- 기존 state를 수정할 수 없다. 기존 state를 복사하고 복사된 값을 변경해 불변성을 지켜야한다.
- 비동기 로직, 랜덤 값 계산 등 side effect를 발생시켜선 안된다.

reducer는 처리할 action인지 확인 후, 처리할 action이 맞을 경우 state의 복사본을 만들어 새로운 값으로 복사본을 업데이트하고 반환한다. 그렇지 않을 경우에는 기존 state를 반환한다.

```javascript
const initialState = { value: 0 };

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/incremented') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1,
    };
  }
  // otherwise return the existing state unchanged
  return state;
}
```

### Store

store는 reducer를 전달받아 생성되며 현재 state 값을 반환하는 `getState` 메소드가 있다.

```javascript
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({ reducer: counterReducer });

console.log(store.getState());
// {value: 0}
```

### Dispatch

Redux store는 `dispatch`라는 메소드를 가지고 있다. state를 업데이트하는 유일한 방법은 `store.dispatch()`를 호출하고 action 객체를 전달하는 것이다. store는 reducer 함수를 실행해 새로운 state 값을 내부에 저장한다. 또한 `getState()`를 통해 업데이트된 값을 확인할 수 있다.

```javascript
store.dispatch({ type: 'counter/incremented' });

console.log(store.getState());
// {value: 1}
```

dispatch는 action을 발생시키는 이벤트 트리거 개념으로 생각하면 된다.

### Action Creator

action creator는 action을 발생시키는 함수이다. `store.dispatch({ type: '...'})`을 통해 action을 발생시킬 수 있지만 동일한 작업이 반복될 경우 비효율적이므로 action creator를 사용해 store에 action을 보낼 수 있다.

```javascript
const doAddToDoItem = (payload) => ({ type: 'TODO_ADDED', payload });

store.dispatch(doAddToDoItem('be awesome today'));
```

`store.dispatch()` 부분을 수행하는 새로운 "wrapper" 함수를 생성함으로써 dispatch 과정을 손쉽게 할 수 있다.

```javascript
// assume we've created a store here
const store = createStore(someReducer);

// and we have our plain action creator function from above
const doAddToDoItem = (payload) => ({ type: 'TODO_ADDED', payload });

// we'd simply need to make a new function that did both things:
const boundActionCreator = (text) => store.dispatch(doAddToDoItem(text));
```

라이브러리에 내장된 `bindActionCreators()` 함수를 사용해 위 "wrapper" 함수를 대체할 수 있다. `bindActionCreators()` 함수는 action creator를 가져와 자동으로 바인딩 해주기 때문에 action의 수가 많을때 유용하다.

### Selector

selelctor는 store state 값에서 특정 부분의 정보를 추출하는 함수이다.

```javascript
const selectCounterValue = (state) => state.value;

const currentValue = selectCounterValue(store.getState());
console.log(currentValue);
// 2
```

## Redux Application Data Flow

![dataflow](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

- #### Redux cycle

  - Action Creator → Action → Dispatch → Reducer → State

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

---

# Redux (part 2)

미들웨어를 사용하면 action이 dispatch된 다음 reducer에 전달되기 전에 추가적인 작업을 수행할 수 있다. 주로 비동기 작업을 위해 자주 사용된다.

## Middleware

미들웨어는 Redux에서 제공되는 `createStore`와 `applyMiddleware`라는 특별한 스토어 인핸서(store inhancer)를 통해 사용할 수 있다.

```javascript
import { createStore, applyMiddleware } from 'redux';
import rootReducer from './reducer';
import { print1, print2, print3 } from './exampleAddons/middleware';

const middlewareEnhancer = applyMiddleware(print1, print2, print3);

// Pass enhancer as the second arg, since there's no preloadedState
const store = createStore(rootReducer, middlewareEnhancer);

export default store;
```

각 미들웨어는 action이 dispatch 될 때 번호를 출력한다.

```javascript
import store from './store';

store.dispatch({ type: 'todos/todoAdded', payload: 'Learn about actions' });
// log: '1'
// log: '2'
// log: '3'
```

미들웨어는 store의 `dispatch` 메소드를 중심으로 파이프라인을 형성한다. `store.dispatch(action)`을 호출하면 파이프라인의 첫번째 미들웨어가 호출되고 작업을 수행할 수 있다. 일반적으로 미들웨어는 reducer와 마찬가지로 실행해야할 action인가를 확인한 후 올바른 action일 경우 사용자가 지정한 작업을 수행한다. 그렇지 않을 경우 다음 미들웨어로 작업을 전달한다.

### Writing Custom Middleware

미들웨어는 세 가지의 중첩된 함수로 작성된다.

```javascript
// Middleware written as ES5 functions

// Outer function:
function exampleMiddleware(storeAPI) {
  return function wrapDispatch(next) {
    return function handleAction(action) {
      // Do anything here: pass the action onwards with next(action),
      // or restart the pipeline with storeAPI.dispatch(action)
      // Can also use storeAPI.getState() here

      return next(action);
    };
  };
}

// ES6 arrow function
const anotherExampleMiddleware = (storeAPI) => (next) => (action) => {
  // Do something in here, when each action is dispatched

  return next(action);
};
```

- `exampleMiddleware` : `applyMiddleware`에 의해 호출되어지는 "미들웨어" 함수이며 스토어의 `{dispatch, getState}`를 포함한 `storeAPI` 객체를 전달 받는다. `dispatch` 함수를 호출하면 미들웨어 파이프라인의 시작으로 action을 보낸다.
- `wrapDispatch` : `next` 인자로 호출된 함수를 받는다. 이 함수가 실제 파이프라인의 시작이되는 미들웨어이다. `next(action)`을 호출해 다음 미들웨어로 전달한다.
- `handleAction` : `action`을 인자로 전달받아 action이 전달 될 때마다 호출된다.

###### + 예제 추가하기

###### + redux-thunk

###### + redux-saga

###### ...

---

# Redux Toolkit

Redux Toolkit은 복잡한 Redux 스토어 구성, Redux 사용을 위한 다수의 패키지 필요성과 많은 코드량을 해결하기 위해 만들어진 개발 도구이다.

Redux Toolkit은 다음과 같은 특징을 가진다.

- simple : 스토어 설정, 리듀서, 불변성 업데이트 로직 등의 기존 Redux의 사용법을 간편하게 사용할 수 있는 유틸리티 기능이 있다.
- opinionated : 스토어 설정에 사용되는 기본 설정이 제공되며 일반적으로 사용되는 Redux addon이 내장되어 있다.
- powerful : 변경 가능한 로직으로 불변성 로직을 작성할 수 있다. state의 전체 slice를 자동으로 생성할 수 있다.
- effective : 적은 코드로 많은 작업을 할 수 있다.

## Installation

### Using Create React App

Redux Toolkit 공식 문서에서 새로운 앱 시작시 권장하는 방법은 CRA용 Redux + TypeScript 템플릿을 사용하는 것이다.

```
// Redux + JS
npx create-react-app app-name --template redux
```

```
// Redux + TS
npx create-react-app app-name --template redux-typescript
```

### Package

모듈 번들러 또는 노드 어플리케이션에서 사용하기 위해서는 npm 패키지를 설치해야한다.

```
npm install @reduxjs/toolkit
// or
yarn add @reduxjs/toolkit
```

## What's Included

Redux Toolkit에 포함된 api는 다음과 같다.

### configureStore()

```javascript
import { configureStore } from '@reduxjs/toolkit';
export const store = configureStore({
  reducer: {
    ex1: ex1Reducer,
    ex2: ex2Reducer,
  },
});
```

`configureStore()`는 Redux의 createStore를 사용해 자동으로 슬라이스 리듀서를 결합한다. Redux 미들웨어와 Redux DevTools을 사용할 수 있다.

### createReducer()

```javascript
const exReducer = createReducer(0, {
  increment: (state, action) => state + action.payload,
});
```

`createReducer()`는 switch문 작성없이 reducer 함수를 사용하는 메소드이다. Immer 라이브러리를 자동으로 사용해 불변성을 유지하면서 state를 업데이트할 수 있다.

첫 번째 인자는 초기 값, 두 번째 인자는 reducer를 가진다.

### createAction()

```javascript
const increment = createAction('increment');
```

`createAction()`은 action type에 따라 action create function을 반환한다.

### createSlice()

```javascript
const exSlice = createSlice({
  name: 'ex',
  initialState: [],
  reducers: {
    action1(state, action) {
      state.ex = action.payload;
    },
  },
});
```

`createSlice()`는 action과 reducer를 모두 가진 메소드이다. action의 이름을 나타내는 `name` 프로퍼티와 초기 값을 받아 slice reducer와 action creator, action type을 생성한다.

## Example

다음은 React, TypeScript, Redux Toolkit을 사용한 카운터 예제이다.

### Store

`configureStore()`를 통해 메소드를 생성하고 `index.ts`에서 `Provider`를 사용해 Redux 스토어를 제공하는 코드이다.

```javascript
// app/store.ts
import { configureStore } from '@reduxjs/toolkit';

export const store = configureStore({
  reducer: {},
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

```javascript
// index.ts
import ReactDOM from 'react-dom';
import App from './App';
import { store } from './app/store';
import { Provider } from 'react-redux';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### Create Redux State Slice

다음은 slice를 생성하는 코드이다. slice 생성을 위해서는 slice 이름, 초기 값, state 업데이트 방법을 정의하는 하나 이상의 reducer 함수가 필요하다. 생성된 슬라이스는 action creator와 전체 slice에 대한 reducer 기능을 export 할 수 있다.

Redux Toolkit의 `createSlice`와 `createReducer`는 Immer 라이브러리를 자동으로 적용해 변경 가능한 로직으로 불변성을 유지하며 업데이트할 수 있다.

```javascript
// app/features/counter/counterSlice.ts

import { createSlice, PayloadAction } from '@reduxjs/toolkit';

export interface State {
  value: number;
}

const initialState: State = {
  value: 0,
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

### Add Slice Reducers to the Store

다음은 slice reducer를 스토어에 추가하는 단계이다.

`configureStore()` 메소드의 `reducer` 프로퍼티에 이전 단계에서 정의하고 export한 reducer import해 추가한다.

```javascript
// app/store.ts

import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

### Use Redux State and Actions in React Components

이제 hooks를 사용해 리액트 구성요소와 Redux 스토어와 인터랙션할 수 있다. `useSelector`와 `useDispatch`를 사용해 Redux 스토어의 state를 가져오고 업데이트할 수 있다.

```javascript
import React from 'react';
import { RootState } from '../../app/store';
import { useSelector, useDispatch } from 'react-redux';
import { decrement, increment } from './counterSlice';

export function Counter() {
  const count = useSelector((state: RootState) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <div>
        <button
          aria-label='Increment value'
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label='Decrement value'
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  );
}
```

###### Note: Redux Toolkit with middlewares

---

**Reference**

- [Redux Fundamentals, Part 2: Concepts and Data Flow](https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow)
- [Action Creators](https://read.reduxbook.com/markdown/part1/04-action-creators.html)
- [Redux Fundamentals, Part 4: Store](https://redux.js.org/tutorials/fundamentals/part-4-store)
- [Redux Toolkit](https://redux-toolkit.js.org/)
