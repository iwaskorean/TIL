#  DOM and Layout Trees

DOM이란 문서 객체 모델(Document Object Model)의 약자로서 문서 내의 요소를 정의하고 요소에 접근하는 방법을 정의하는 모델이다.

<br>

### DOM(Document Object Model)

브라우저는 문서를 읽은 후 DOM이라는 표현 트리(representational tree)를 만들고 트리에 접근 가능한 방법을 정의한다. 즉, DOM은 HTML/XML 문서를 표현하기 위한 인터페이스이며 구조화 된 표현이다.

<br>

### Advantages

DOM의 아래와 같은 장점을 가진다.

- 새로고칠 필요 없이 페이지의 데이터를 업데이트 할 수 있다.

- 사용자가 커스터마이징한 앱을 만들고 새로고침 없이 페이지 레이아웃을 변경 할 수 있다.

- 드래그, 이동, 삭제 등 요소들을 조작 할 수 있다.

<br>

### Representation

![img](https://cdn-media-1.freecodecamp.org/images/3n6SPcMH0mmG6cFeB3SJBEA-9Yyfgp3xYZ7u)

위 이미지는 브라우저가 문서를 읽은 후 생성한 노드로 구성된 표현트리이다. 요소가 DOM에 배치되는위치를 노드(node)라고 한다.

또한 트리는 아래와 같은 요소들로 구성된다.

- Document : HTML 문서를 다룬다.

- Element : HTML 이나 XML 내부의 태그들을 DOM 요소로 변환한다.

- Text : 태그들의 내용(content)

- Attribute : 특정한 HTML 요소가 가지고 있는 속성

<br>

### To Access Elements

ID, Class, HTML 태그 및 선택자를 이용해 DOM의 요소에 접근할 수 있다.

| Gets              | Method                     |
| ----------------- | -------------------------- |
| ID                | `getElementById()`         |
| Class             | `getElementsByClassName()` |
| Tag               | `getElementsByTagName()`   |
| Selector (single) | `querySelector()`          |
| Selector (all)    | `querySelectorAll()`       |

<br>

### To Traverse the DOM

프로퍼티를 이용해 HTML 문서의 루트 노드 및 부모/자식/형제의 속성을 이용해 접근할 수 있다.

- 루트 노드

  | Property                   | Node        | Node Type       |
  | -------------------------- | ----------- | --------------- |
  | `document`                 | `#document` | `DOCUMENT_NODE` |
  | `document.documentElement` | `html`      | `ELEMENT_NODE`  |
  | `document.head`            | `head`      | `ELEMENT_NODE`  |
  | `document.body`            | `body`      | `ELEMENT_NODE`  |

- 부모 노드

  | Property        | Gets                |
  | --------------- | ------------------- |
  | `parentNode`    | Parent Node         |
  | `parentElement` | Parent Element Node |

- 자식 노드

  | Property            | Gets                     |
  | ------------------- | ------------------------ |
  | `childNodes`        | Child Nodes              |
  | `firstChild`        | First Child Node         |
  | `lastChild`         | Last Child Node          |
  | `children`          | Element Child Nodes      |
  | `firstElementChild` | First Child Element Node |
  | `lastElementChild`  | Last Child Element Node  |

- 형제 노드

  | Property                 | Gets                          |
  | ------------------------ | ----------------------------- |
  | `previousSibling`        | Previous Sibling Node         |
  | `nextSibling`            | Next Sibling Node             |
  | `previousElementSibling` | Previous Sibling Element Node |
  | `nextElementSibling`     | Next Sibling Element Node     |

<br>

### To Make Changes to the DOM

메소드와 프로퍼티를 이용해 노드를 추가, 변경, 교체 및 삭제할 수 있다.

- 노드 생성

  | Property/Method    | Description                                    |
  | ------------------ | ---------------------------------------------- |
  | `createElement()`  | Create a new element node                      |
  | `createTextNode()` | Create a new text node                         |
  | `node.textContent` | Get or set the text content of an element node |
  | `node.innerHTML`   | Get or set the HTML content of an element      |

- 노드 삽입

  | Property/Method       | Description                                                  |
  | --------------------- | ------------------------------------------------------------ |
  | `node.appendChild()`  | Add a node as the last child of a parent element             |
  | `node.insertBefore()` | Insert a node into the parent element before a specified sibling node |
  | `node.replaceChild()` | Replace an existing node with a new node                     |

- 노드 삭제

  | Method               | Description       |
  | -------------------- | ----------------- |
  | `node.removeChild()` | Remove child node |
  | `node.remove()`      | Remove node       |

<br>

<br>


------

**Reference**

- [Understanding the DOM — Document Object Model](https://www.digitalocean.com/community/tutorial_series/understanding-the-dom-document-object-model)
- [What’s the Document Object Model, and why you should know how to use it.](https://www.freecodecamp.org/news/whats-the-document-object-model-and-why-you-should-know-how-to-use-it-1a2d0bc5429d/)
