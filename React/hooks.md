# Hooks

Hook은 React 16.8 버전부터 새롭게 추가된 상태 및 생명주기 관리 API이다. Hook을 사용하면 클래스 컴포넌트의 사용 없이 함수형 컴포넌트 내부에서 상태 저장이 가능하며 생명주기 관리를 할 수 있다.

<br>

## useState

useState는 상태 관리를 위한 Hook이다.



### Create initial state

```javascript
const [<상태 저장 변수>, <상태 갱신 함수>] = useState(<상태 초기 값>);
```

초기 상태 생성시 상태 저장 변수의 초기 값을  array, number, boolean 등 다양한 타입 및 값으로 지정할 수 있다. 또한 useState는 값이 아닌 함수를 인자로 가질수 있는데 이 경우, 첫 렌더링될때만 실행된다.

<br>

### Update State

```javascript
const [count, setCount] = useState(0)

setCount(prevCount => prevCount + 1)
```

초기 상태 생성시 선언한 상태 갱신 함수를 통해 상태 저장 변수의 값을 갱신할 수 있다.

<br>

### Example

useState hook과 상태 저장 변수 count를 사용한 카운터 예제

```javascript
function Counter() {
  const [count, setCount] = useState(0) // creating initial state

  function changeCount(amount) {
    setCount(prevCount => prevCount + amount)
  }

  function resetCount() {
    setCount(0)
  }

  return (
    <>
      <span>{count}</span>
      <button onClick={() => changeCount(1)}>+</button>
      <button onClick={() => changeCount(-1)}>-</button>
      <button onClick={() => resetCount()}>Reset</button>
    </>
  )
}
```

<br>

<br>

## useEffect

![i](https://user-images.githubusercontent.com/37759759/65383863-16be3180-dd56-11e9-9771-46a40ba34569.png)



```javascript
useEffect(function, deps)
```

useEffect는 클래스 컴포넌트의 라이프사이클 메소드를 대체하며 컴포넌트가 렌더링시 혹은 렌더링 이후 특정 작업을 수행하도록하는 hook이다.

<br>

### Creating Your First Side Effect

네트워크 요청, 데이터 가져오기, 구독 설정, 수동으로 컴포넌트의 DOM을 수정하는 등 현재 함수의 범위에 벗어난 것에 영향을 끼치는 것을 side effect라고 한다. useEffect는 이러한 side effect를 처리를 위해 사용된다.



useEffect를 사용하는 가장 기본적인 방법은 단일 함수(single function)를 전달하는 것이다.

```javascript
useEffect(() => {
  console.log('This is a side effect')
})
```

이 경우 컴포넌트가 처음 마운트될 때, props가 변경 될 때, 상태가 변경 될 때 등 렌더링 될 때마다 실행된다. 그러나 마운트 단계에서만 side effect가 필요하거나 특정 props나 상태가 변경되는 경우에는 이상적이지 않다. 

두번째 매개변수인 의존성 배열을 사용하면 이전 렌더링의 의존성과 비교해 마지막 렌더링 이후 의존성이 변경된 경우 side effect가 실행된다. 마운트 단계에서만 실행하고 싶다면 빈 배열을 두번째 매개 변수로 전달하면 된다.

```javascript
useEffect(() => {
  console.log('Only run on mount')
}, [])


useEffect(() => {
  console.log('Only run on url change')
}, [url])
```

<br>

### Cleaning Up Side Effects

언마운트 이전, 업데이트 직전에 side effect가 필요하다면 clean up 함수를 반환해야한다.

```javascript
useEffect(() => {
  console.log('This is my side effect')

  return () => {
    console.log('This is my clean up')
  }
})
```

위와 같은 useEffect를 포함한 컴포넌트가 마운트 후 리렌더링 2회 그리고 언마운트 될 경우 아래와 같이 콘솔 출력하게 된다.

```
// MOUNTED
// This is my side effect

// RE-RENDER 1:
// This is my clean up
// This is my side effect

// RE-RENDER 2:
// This is my clean up
// This is my side effect

// UN-MOUNT:
// This is my clean up
```

<br>

<br>

## useMemo & useCallback

useMemo와 useCallback hook을 이해하기위해서는 memoization에 대해 알아야한다.

<br>

### What is Memoization?

Memoization은 본질적으로 캐싱과 같은 의미이다. 동일한 계산을 반복하는 함수를 실행할때 값을 다시 계산하는 대신 캐시된 값을 사용해 처리 속도를 빠르게 향상시키는 것이 memoization이다. 


```javascript
const cache = {}

function slow(a) {
  if (cache[a]) return cache[a]
  
  const result = /* Complex logic */
  cache[a] = result
  return result
}
```

렌더링 될 때마다 모든 컴포넌트 로직이 재계산되고 로직의 계산속도가 느린 경우 급격한 속도저하가 일어나는 매우 일반적인 문제를 해결하기 위한 hook이 바로 useMemo와 useCallback이다.

<br>

### useMemo

useMemo는 useEffect와 비슷한 방식으로 동작하며 두번째 인자인 의존성이 변경되었을 때만 memoization된 값만 다시 계산한다. 즉, useMemo는 memoization된 값을 반환한다.

```javascript
const result = useMemo(() => {
  return slowFunction(a)
}, [a])
```

useMemo로 전달된 함수는 렌더링 중에 실행된다. 두번째 인자가 없을 경우 렌더링 될 때마다 새 값을 계산한다.

<br>

### useCallback

useCallback은 의존성 배열을 기반으로 결과를 캐시하기 때문에 useMemo와 동일하게 작동한다. 그러나 useCallback은 캐싱된 값이 아닌 캐싱된 함수를 위해 사용된다. 즉, useCallback은 memoization된 콜백을 반환한다.

```javascript
const handleReset = useCallback(() => {
  return doSomething(a, b)
}, [a, b])
```

구문상은 useMemo와 동일하지만 useMemo는 의존성이 변경될 때마다 전달된 함수를 호출한 후 호출에 대한 값을 반환한다. 반면에 useCallback은 의존성이 변경 될 때마다 함수를 호출하지않고 함수의 새로운 버전을 반환한다. 

```javascript
useCallback(() => {
  return a + b
}, [a, b])

useMemo(() => {
  return () => a + b
}, [a, b])
```

위 코드와 같이 둘 다 동일한 값을 반환하지만, useCallback은 전달된 함수를 반환하고 useMemo는 전달된 함수의 결과를 반환한다.

<br>

useMemo와 마찬가지로 useEffect도 referential equality를 유지하기위해 사용된다.

```javascript
function Parent() {
  const [items, setItems] = useState([])
  const handleLoad = (res) => setItems(res)

  return <Child onLoad={handleLoad} />
}

function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad)
  }, [onLoad])
}
```

위 예제에서 `handleLoad` 함수는 `Parent` 컴포넌트가 리렌더링 될 때마다 re-create된다. 이것은 `onLoad` 함수가 매 렌더링마다 다른 referential equality를 가지고 있기 때문에 `Child` 컴포넌트의 useEffect가 재실행된다는 것을 의미한다. 

```javascript
function Parent() {
  const [items, setItems] = useState([])
  const handleLoad = useCallback((res) => setItems(res), [])

  return <Child onLoad={handleLoad} />
}

function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad)
  }, [onLoad])
}
```

`handleLoad`에 useCallback를 사용함으로써 `handleLoad` 함수는 변경되지 않고,  `Child` 컴포넌트의 useEffect도 렌더링 될때마다 호출되지 않는다.

<br>

## useRef

...

<br>

## useContext

..

<br>

<br>

------

**Reference**

- [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html#usecallback)
- [Everything You Need To Know About useState](https://blog.webdevsimplified.com/2020-04/use-state/)
- [Everything You Need To Know About useEffect](https://blog.webdevsimplified.com/2020-04/use-effect/)
- [How To Use Memoization To Drastically Increase React Performance](https://blog.webdevsimplified.com/2020-05/memoization-in-react/)