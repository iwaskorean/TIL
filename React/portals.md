# Portals

리액트의 렌더링 시스템은 표준 자바스크립트보다 훨씬 쉽게 컨텐츠들을 동적으로 작업할 수 있도록 설계되었다. 그러나 부모 컴포넌트 외부에서 콘텐츠를 동적으로 렌더링하는데는 어려움이 있다.

portal은 컴포넌트 트리에 의해 정의된 DOM 계층 구조 외부에서 컴포넌트를 렌더링하는 기능이다. portal을 사용하면 컨텐츠를 부모 컴포넌트 외부에서 렌더링할 수 있다.

<br>

### Example without using React Portal

portal을 사용하지 않고 modal 기능을 만드는 예제이다.

```javascript
// App.js

import React, { useState } from 'react';
import Modal from './Modal';

const BUTTON_WRAPPER_STYLES = {
  position: 'relative',
  zIndex: 1,
};

const OTHER_CONTENT_STYLES = {
  position: 'relative',
  zIndex: 2,
  backgroundColor: 'red',
  padding: '10px',
};

export default function App() {
  const [isOpen, setIsOpen] = useState(false);
  return (
    <>
      <div
        id="button-wrapper"
        style={BUTTON_WRAPPER_STYLES}
        onClick={() => console.log('clicked')}
      >
        <button onClick={() => setIsOpen(true)}>Open Modal</button>

        <Modal open={isOpen} onClose={() => setIsOpen(false)}>
          <p id="modal-message">Modal</p>
        </Modal>
      </div>

      <div style={OTHER_CONTENT_STYLES}>Other Content</div>
    </>
  );
}
```

```javascript
// Modal.js
import React from 'react';

const MODAL_STYLES = {
  position: 'fixed',
  top: '50%',
  left: '50%',
  transform: 'translate(-50%, -50%)',
  backgroundColor: '#FFF',
  padding: '50px',
  zIndex: 1000,
};

const OVERLAY_STYLES = {
  position: 'fixed',
  top: 0,
  left: 0,
  right: 0,
  bottom: 0,
  backgroundColor: 'rgba(0, 0, 0, .7)',
  zIndex: 1000,
};

export default function Modal({ open, children, onClose }) {
  if (!open) return null;

  return (
    <>
      <div id="modal-overay" style={OVERLAY_STYLES} />
      <div id="modal-main" style={MODAL_STYLES}>
        <button id="button-close" onClick={onClose}>
          Close Modal
        </button>
        {children}
      </div>
    </>
  );
}
```

`App.js`에서 `Modal` 컴포넌트를 렌더링했을 때의 결과는 아래와 같다.

```HTML
<div id="root">
  <!-- button for modal open and modal content -->
  <div id="button-wrapper" style="...">
    <button>Open Modal</button>
    <div id="modal-overay" style="..."></div>

    <div id="modal-main" style="...">
      <button id="button-close">Close Modal</button>
      <p id="modal-message">Modal</p>
    </div>
  </div>

  <!-- other content -->
  <div id="other-content" style={OTHER_CONTENT_STYLES}>Other Content</div>
</div>
```

`Modal` 컴포넌트는 `<div id="button-wrapper">` 요소의 하위 요소이므로 `Modal` 컴포넌트의 `<div id="modal-overay">` 요소는 `z-index:1000`임에도 불구하고 `<div="other-content">` 요소에 영향을 주지 않는다.

<br>

### Example using React portal

portal은 `ReactDOM.createPortal()` 함수를 통해 사용할 수 있다.

 `ReactDOM.createPortal()` 함수의 첫 번째 인자에는 리액트 요소, 배열, Fragment, 문자열 등 렌더링될 수 있는 모든 요소들이며, 두 번째 인자는 DOM 요소이다.

```javascript
// Modal.js
export default function Modal({ open, children, onClose }) {
  if (!open) return null;

  return ReactDom.createPortal(
    <>
      <div style={OVERLAY_STYLES} />
      <div style={MODAL_STYLES}>
        <button onClick={onClose}>Close Modal</button>
        {children}
      </div>
    </>,
    document.body
  );
}
```

```javascript
<div id="root">
  <div id="button-wrapper" style="...">
    <button>Open Modal</button>
  </div>

  <div id="other-content" style="...">Other Content</div>
</div>

<div id="modal-overay" style="..."></div>
<div id="modal-main" style="...">
  <button>Close Modal</button>
  <p id="modal-message">Modal</p>
</div>
```

`App.js`에서 portal이 적용된 `Modal` 컴포넌트를 렌더링하면 `Modal` 컴포넌트의 요소들이 `<div id="button-wrapper">` 요쇼의 하위 요소가 아닌 `document.body` 스코프에 렌더링되어 `<div id="modal-overay">` 요소가 `<div id="other-content">` 요소에 영향을 끼치게 된다.

<br>

<br>

------

**Reference**

- [The Forgotten React Renderer - React Portal](https://blog.webdevsimplified.com/2019-12/how-to-use-react-portal/)
- [Portals](https://ko.reactjs.org/docs/portals.html)