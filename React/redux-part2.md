# Redux (part 2)

미들웨어를 사용하면 action이 dispatch된 다음 reducer에 전달되기 전에 추가적인 작업을 수행할 수 있다. 주로 비동기 작업을 위해 자주 사용된다.

<br>

## Middleware

미들웨어는 Redux에서 제공되는 `createStore`와 `applyMiddleware`라는 특별한 스토어 인핸서(store inhancer)를 통해 사용할 수 있다.

```javascript
import { createStore, applyMiddleware } from 'redux'
import rootReducer from './reducer'
import { print1, print2, print3 } from './exampleAddons/middleware'

const middlewareEnhancer = applyMiddleware(print1, print2, print3)

// Pass enhancer as the second arg, since there's no preloadedState
const store = createStore(rootReducer, middlewareEnhancer)

export default store
```

각 미들웨어는 action이 dispatch 될 때 번호를 출력한다.

```javascript
import store from './store'

store.dispatch({ type: 'todos/todoAdded', payload: 'Learn about actions' })
// log: '1'
// log: '2'
// log: '3'
```



미들웨어는 store의 `dispatch` 메소드를 중심으로 파이프라인을 형성한다.  `store.dispatch(action)`을 호출하면 파이프라인의 첫번째 미들웨어가 호출되고 작업을 수행할 수 있다. 일반적으로 미들웨어는 reducer와 마찬가지로 실행해야할 action인가를 확인한 후 올바른 action일 경우 사용자가 지정한 작업을 수행한다. 그렇지 않을 경우 다음 미들웨어로 작업을 전달한다.

<br>

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

      return next(action)
    }
  }
}

// ES6 arrow function
const anotherExampleMiddleware = storeAPI => next => action => {
  // Do something in here, when each action is dispatched

  return next(action)
}
```

- `exampleMiddleware` : `applyMiddleware`에 의해 호출되어지는 "미들웨어" 함수이며 스토어의 `{dispatch, getState}`를 포함한 `storeAPI` 객체를 전달 받는다. `dispatch` 함수를 호출하면 미들웨어 파이프라인의 시작으로 action을 보낸다.
- `wrapDispatch` : `next` 인자로 호출된 함수를 받는다. 이 함수가 실제 파이프라인의 시작이되는 미들웨어이다. `next(action)`을 호출해 다음 미들웨어로 전달한다.
- `handleAction` : `action`을 인자로 전달받아 action이 전달 될 때마다 호출된다.

<br>

###### +  예제 추가하기 

###### +  redux-thunk

###### +  redux-saga

###### ...

<br>

<br>


------

**Reference**

- [Redux Fundamentals, Part 4: Store](https://redux.js.org/tutorials/fundamentals/part-4-store)
