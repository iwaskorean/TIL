# Redux Toolkit

Redux Toolkit은 복잡한 Redux 스토어 구성, Redux 사용을 위한 다수의 패키지 필요성과 많은 코드량을 해결하기 위해 만들어진 개발 도구이다.

Redux Toolkit은 다음과 같은 특징을 가진다.

- simple : 스토어 설정, 리듀서, 불변성 업데이트 로직 등의 기존 Redux의 사용법을 간편하게 사용할 수 있는 유틸리티 기능이 있다.
- opinionated : 스토어 설정에 사용되는 기본 설정이 제공되며 일반적으로 사용되는 Redux addon이 내장되어 있다.
- powerful : 변경 가능한 로직으로 불변성 로직을 작성할 수 있다. state의 전체 slice를 자동으로 생성할 수 있다.
- effective : 적은 코드로 많은 작업을 할 수 있다.

<br>

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

<br>

### Package

모듈 번들러 또는 노드 어플리케이션에서 사용하기 위해서는 npm 패키지를 설치해야한다.

```
npm install @reduxjs/toolkit
// or
yarn add @reduxjs/toolkit
```

<br>

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
 increment : (state, action) => state + action.payload
})
```

`createReducer()`는 switch문 작성없이 reducer 함수를 사용하는 메소드이다.  Immer 라이브러리를 자동으로 사용해 불변성을 유지하면서 state를 업데이트할 수 있다. 

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

<br>

## Example

다음은 React, TypeScript, Redux Toolkit을 사용한 카운터 예제이다.

### Store

`configureStore()`를 통해 메소드를 생성하고 `index.ts`에서 `Provider`를 사용해 Redux 스토어를 제공하는 코드이다.

```javascript
// app/store.ts
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```

```javascript
// index.ts
import ReactDOM from 'react-dom'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

<br>

### Create Redux State Slice

다음은 slice를 생성하는 코드이다. slice 생성을 위해서는 slice 이름, 초기 값, state 업데이트 방법을 정의하는 하나 이상의 reducer 함수가 필요하다. 생성된 슬라이스는 action creator와 전체 slice에 대한 reducer 기능을 export 할 수 있다. 

Redux Toolkit의 `createSlice`와 `createReducer`는 Immer 라이브러리를 자동으로 적용해 변경 가능한 로직으로 불변성을 유지하며 업데이트할 수 있다.

```javascript
// app/features/counter/counterSlice.ts

import { createSlice, PayloadAction } from '@reduxjs/toolkit'

export interface State {
  value: number
}

const initialState: State = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload
    },
  },
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

<br>

### Add Slice Reducers to the Store

다음은 slice reducer를 스토어에 추가하는 단계이다. 

`configureStore()` 메소드의 `reducer` 프로퍼티에 이전 단계에서 정의하고 export한 reducer import해 추가한다.

```javascript
// app/store.ts

import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

<br>

### Use Redux State and Actions in React Components

이제 hooks를 사용해 리액트 구성요소와 Redux 스토어와 인터랙션할 수 있다. `useSelector`와 `useDispatch`를 사용해 Redux 스토어의 state를  가져오고 업데이트할 수 있다.

```javascript
import React from 'react'
import { RootState } from '../../app/store'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

export function Counter() {
  const count = useSelector((state: RootState) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```

<br>

###### Note: Redux Toolkit with middlewares

<br>

<br>

------

**Reference**

- [Redux Toolkit](https://redux-toolkit.js.org/)
