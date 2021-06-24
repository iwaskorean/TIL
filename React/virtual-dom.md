# Virtual Dom

DOM은 트리 구조로 표현되기 때문에 DOM을 빠르게 변경하고 업데이트 할 수 있지만 변경된 후에 UI를 업데이트하기 위해서는 업데이트된 요소와 하위 요소들을 다시 렌더링 해야한다. 따라서 UI 구성요소가 많을수록 DOM 업데이트로 인한 리렌더링 횟수가 많아져 DOM 조작이 느려지고 비용이 증가하게 된다.

이것이 가상 DOM 개념이 등장하게 된 배경이다.

<br>

### Virtual Dom

가상 DOM은 말 그대로 DOM의 "가상" 표현(virtual representation)이다. 어플리케이션의 상태가 변경될 때마다 DOM 대신 가상 DOM이 업데이트된다.

<br>

### How is Virtual DOM faster?

새로운 요소가 UI에 추가되면 트리 형태의 가상 DOM이 생성된다. 각 요소는 이 트리의 노드이며 이러한 요소들의 상태가 변경되면 새로운 가상 DOM 트리가 생성된다. 새롭게 생성된 가상 DOM 트리는 이전 가상 DOM 트리와의 비교 과정을 거치게 된다.

이 과정이 끝나면 가상 DOM은 실제 DOM에 변경을 수행할 수 있는 최적의 방법을 계산한다. 이를 통해 실제 DOM의 업데이트를 최소한의 작업으로 실행해 실제 DOM의 업데이트 성능 비용을 줄이게 된다.

<br>

![IMG](https://i0.wp.com/programmingwithmosh.com/wp-content/uploads/2018/11/lnrn_0201.png?w=1173&ssl=1)

위 그림에서 빨간 원은 변경된 노드 즉, 상태가 변경된 요소를 나타낸다. 이전 버전의 가상 DOM 트리와 현재의 가상 DOM 트리의 차이를 감지한 다음, 변경된 요소와 그 요소의 하위트리 전체가 리렌더링 되어 업데이트된 UI를 제공한다. 이 업데이트된 가상 DOM 트리는 실제 DOM으로 업데이트된다.

<br>

### How does React use Virtual DOM

리액트의 UI는 컴포넌트로 구성되며 각 컴포넌트에는 state가 있다. 컴포넌트의 state가 변경되면 리액트는 가상 DOM 트리를 업데이트하며 업데이트된 현재의 가상 DOM과 이전 버전의 가상 DOM을 비교한다. 이 과정을 "diffing"이라고 한다.

리액트가 변경된 가상 DOM 객체를 확인하게되면 실제 DOM에서 변경된 해당 객체만 업데이트한다.  이 과정은 실제 DOM을 직접 조작하는것보다 훨씬 향상된 성능을 가진다.

<br>

### React render() function

`render()` 함수는 라이프 사이클 메소드이자 UI가 업데이트되고 렌더링되는 곳이다.  또한 `render()` 함수는 리액트 요소의 트리가 생성되는 엔트리 포인트이다.  

컴포넌트의 state나 props가 업데이트되면 `render()` 함수는 리액트 요소들로 구성된 트리를 반환한다. 예를 들어, `setState()`를 호출해 state의 값을 변경하면 리액트는 즉시 state의 변화를 감지하고 컴포넌트를 다시 렌더링한다.

위 과정을 거친후 리액트는 가장 최근 트리의 변경 사항과 일치하도록 효율적으로 UI를 업데이트하는 방법을 알아낸다.

이것이 리액트가 가상 DOM을 먼저 변경하고 실제 DOM에서 변경된 객체만 업데이트하는 과정이다.

<br>

### Batch Update

리액트는 batch 업데이트 메커니즘으로 실제 DOM을 업데이트한다. 상태가 변경될 때마다 매번 업데이트를 보내는 대신 실제 DOM에 대한 업데이트를 일괄적(batch)으로 전송해 성능을 향상시킨다.

UI를 다시 출력하는 것(repainting)은 가장 많은 비용을 차지하는 부분이며 리액트는 UI의 repainting을 위해 실제 DOM이 batch 업데이트만 수신하도록 제한한다.

<br>

### Recap

- 잦은 DOM 조작은 많은 비용이 요구되며 heavy한 퍼포먼스를 가진다.
- 가상 DOM은 실제 DOM의 가상 표현이다.
- state가 변경될 때, 가상 DOM은 업데이트되고 이전 가상 DOM과 현재 버전의 가상 DOM을 비교하는 과정을 "diffing"이라고 한다.
- 가상 DOM은 일괄(batch) 업데이트를 실제 DOM에게 전송해 UI를 업데이트한다.
- 리액트는 가상 DOM을 사용해 성능을 향상시킨다.
- observable을 사용해 state와 props의 변경을 감지한다.
- 리액트는 효율적은 diff 알고리즘을 사용해 가상 DOM의 버전을 비교한다.
- 비교 후 UI의 repainting 또는 리렌더링을 위해 일괄 업데이트가 실제 DOM으로 전송되는지 확인한다.

<br>

<br>

------

**Reference**

- [React Virtual DOM Explained in Simple English](https://programmingwithmosh.com/react/react-virtual-dom-explained/)