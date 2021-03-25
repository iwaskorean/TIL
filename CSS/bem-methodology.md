# BEM Methodology

<br>

### BEM Methodology(BEM Naming Convention)

- Block, Element, Modifier 방법론은 HTML와 CSS간의 관계를 잘 이해 할 수 있게 하기 위한 class 네이밍 규칙이다.
- BEM 방법론에서는 `ID` selector는 사용하지 않는다.

<br><br>

![img](https://devopedia.org/images/article/152/2881.1549627976.png)

<br>

### Block

- 기능적으로 재사용 가능함 독립적인 페이지 구성 요소

- 구성 요소들 중 top-level에 위치

- block의 class name은 상태가 아닌 목적/용도(What is it?)를 나타낸다.

- block들간 서로  nested한 형태를 갖출 수 있다.

  ```
  <!-- `header` block -->
  <header class="header">
      <!-- Nested `logo` block -->
      <div class="logo"></div>
  
      <!-- Nested `search-form` block -->
      <form class="search-form"></form>
  </header>
  ```

<br>

### Element

- 재사용될 수 없는 block의 요소

- element는 상태가 아닌 목적/용도를 나타낸다.

- block과 element는 `__`로 구분한다.

  - `blockName__elementName`

- element는 항상 block의 부분이며, 다른 element의 부분이 될 수 없다.

- element들간 nested한 형태를 갖출 수 있다.

  ```
  <form class="search-form">
    <div class="search-form__content">
        <input class="search-form__input"/>
        <button class="search-form__button">Search</button>
    </div>
  </form>
  ```

  ```
  <div class="block">
      <div class="block__elem1">
          <div class="block__elem1__elem2">
              <div class="block__elem1__elem2__elem3"></div>
          </div>
      </div>
  </div>
  
  // element는 다른 element의 부분이 될 수 없으므로
  // 위 코드를 올바르게 고치면
  
  <div class="block">
      <div class="block__elem1">
          <div class="block__elem2">
              <div class="block__elem3"></div>
          </div>
      </div>
  </div>
  ```

<br>

### Modifier

- modifier는 어떻게 보이는가, 상태, 어떠한 작동방식을 가졌는가 등을 나타낸다.

- `_` 또는 `--`로 구분한다. (주로 `--` 사용)

- modifier는 단독으로 사용 될 수 없다.

- `disable`나 `focused`와 같은 Boolean Type, `theme`의 종류나 `size` 등의 Key-Value Type이 있다.

  ```
  <ul class="tab">
    <li class="tab__item tab__item--focused">01</li>
    <li class="tab__item tab__item--size-large">02</li>
    <li class="tab__item tab__item--color-red">03</li>
  </ul>
  ```

<br>

### SASS and BEM

```
<div class="block block--modifier">
	<div class="block__element">
</div>
```

- 위와 같은 HTML 코드가 있을때, SASS 선택자로 작성할 경우

```
// CSS
.block {
}
.block__element {
}
.block--modifier {
}

// SASS
.block{
	&__element{
	}
	&--modifier{
	}
}
```

<br>

<br>



## Example

![img](https://media.merixstudio.com/uploads/block2.png)

![img2](https://media.merixstudio.com/uploads/element2.png)

![img3](https://media.merixstudio.com/uploads/modifier2.png)

```
<a href="#" class="btn">
  <span class="btn__name">Default</span>
  <span class="btn__counter">0</span>
</a>

<a href="#" class="btn btn--positive">
  <span class="btn__name">Positive</span>
  <span class="btn__counter">2</span>
</a>

<a href="#" class="btn btn--negative">
  <span class="btn__name">Negative</span>
  <span class="btn__counter">-2</span>
</a>
```

<br>

<br>

<br>




------

**Reference**

- [BEM 101](https://css-tricks.com/bem-101/)
- [BEM Methodology Documentation](https://en.bem.info/methodology/quick-start/)
- [BEM it! Introduction to BEM methodology](https://www.merixstudio.com/blog/bem-code-organization-methodology/)