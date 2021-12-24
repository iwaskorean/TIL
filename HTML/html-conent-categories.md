# HTML Conent Categories

![](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories/content_categories_venn.png)

HTML 요소들은 콘텐츠 모델을 가지고 있다. 콘텐츠 모델이란 요소가 가지는 성격과 특성에 따라 분류해놓은 규칙이자 포괄적 개념이다.

<br>

## Contents

### Flow Content

- body에 포함할 수 있는 요소이다.
- base, style, title 요소를 제외한 나머지 모든 요소이다.

```
a, abbr, address, area, map, article, aside, audio, b, bdi, bdo,
  blockquote, br, button, canvas, cite, code, data, datalist, del,
  details, dfn, dialog, div, dl, em, embed, fieldset, figure,
  footer, form, h1, h2, h3, h4, h5, h6, header, hgroup, hr, i,
  iframe, img, input, ins, kbd, label, link, main, map, mark, math,
  menu, meta, meter, nav, noscript, object, ol, output, p, picture,
  pre, progress, q, ruby, s, samp, script, section, select, slot,
  small, span, strong, sub, sup, svg, table, template, textarea,
  time, u, ul, var, video, wbr, autonomous custom elements, text
```

### Metadata Content

- 콘텐츠와 문서를 구조화하는 요소이다.
- 다른 콘텐츠의 표시, 동작, 관계 등을 설정한다.
- 화면에 출력하지 않기 때문에 `display:none` 방식으로 출력된다.

```
base, link, meta, noscript, script, style, template, title
```

- link, meta, noscript, scraipt, template 요소는 경우에 따라 플로우 콘텐츠이다.

### Heading Content

- 섹셔닝 콘텐츠의 헤더이다.
- 섹셔닝 콘텐츠가 없어도 헤딩 콘텐츠가 있으면 암시적으로 섹션이 형성된다.

```
  h1, h2, h3, h4, h5, h6, hgroup
```

### Sectioning Content

- 문서의 개요를 형성한다.
- 헤딩, 헤더, 풋터의 범위이다.
- 섹셔닝 콘텐츠는 암시적인 개요를 형성하며 헤딩 콘텐츠를 함께 사용하면 명시적인 개요를 형성한다.

```
article, aside, nav, section
```

### Phrasing Content

- 구문 콘텐츠, 단락을 형성하는 콘텐츠이다.
- link, meta, noscript, script, template는 `display: none`이 적용된다.

```
  a, abbr, area, audio, b, bdi, bdo, br, button, canvas, cite, code,
  data, datalist, del, dfn, em, embed, i, iframe, img, input, ins,
  kbd, label, link, map, mark, math, meta, meter, noscript, object,
  output, picture, progress, q, ruby, s, samp,script, select, slot,
  small, span, strong, sub, sup, svg, template, textarea, time, u,
  var, video, wbr, autonomous custom elements, text
```

### Embedded Content

- HTML, CSS가 아닌 멀티미디어 요소, 외부 자원을 참조하는 콘텐츠이다.
- 모든 임베디드 콘텐츠는 구문 콘텐츠이다.
- `display: inline | inline-block`이 적용된다.

```
  audio, canvas, embed, iframe, img, math, object, picture, svg,
  video
```

### Interactive Content

- 사용자와 상호 작용할 수 있는 콘텐츠이다.
- 키보드, 마우스 등의 입력 장치로 조작할 수 있다.
- `display: inline | inline-block`이 적용된다.

```
  a, audio, button, details, embed, iframe, img, input, label,
  select, textarea, video
```

### Palpable Content

- 구체적으로 보여지고 이해할 수 있는 콘텐츠 요소이다.

```
a, abbr, address, article, aside, audio, b, bdi, bdo, blockquote,
  button , canvas, cite, code, data, details, dfn, div, dl, em,
  embed, fieldset, figure, footer, form, h1, h2, h3, h4, h5, h6,
  header, hgroup, i, iframe, img, input, ins, dbd, label, main, map,
  mark, math, menu, meter, nav, object, ol, output, p, pre,
  progress, g, ruby, s, samp, section, select, small, span, strong,
  sub, sup, svg, table, textarea, time, u, ul, var, video,
  autonomous custom elements, text that is not inter-element
  whitespace
```

### Script-supporting Element

- 렌더링하지 않지만 사용자에게 기능을 제공하는 요소이다.

```
script, template
```

### Transparent Content Models

- 투명 콘텐츠 모델이며 부모의 콘텐츠 모델을 따른다.
- 투명한 요소를 제거해도 부모와 자식 관계가 문법적으로 유효해야 한다.

```
a, ins, del, object, video, audio, map, noscript, canvas
```

<br>

## Example

다음 코드의 HTML 요소들은 유효할까?

```html
<p>
  <a>
    <div></div>
  </a>
</p>
```

정답은 ''유효하지 않다''이다. 그 이유는 다음과 같다.

```html
<p>
  <!-- p는 프레이징 콘텐츠이다. -->
  <a>
    <!-- a는 플로우, 프레이징, 인터랙티브, 팔퍼블 콘텐츠이다.-->
    <div><!-- div는 플로우, 팔퍼블 콘텐츠이다. --></div>
  </a>
</p>
```

- p 요소를 제외하고 보면 a 요소는 div 요소를 자식 요소로 가질 수 있으므로 유효한 코드이다.
- 그러나 a 요소는 투명한 요소이기 때문에 부모의 콘텐츠 모델을 따른다.
- a 요소는 부모 요소인 p 요소의 프레이징 콘텐츠를 따르며 프레이징 콘텐츠는 자식 요소로 프레이징 콘텐츠만을 허용한다.
- div 요소는 플로우, 팔퍼블 콘텐츠 카테고리에 속한다.

위와 같은 이유로 예제 코드의 요소들은 유효하지 않다. a 요소와 같은 투명한 요소를 사용할 때 주의해야 한다.

---

### Reference

- [HTML Living Standard](https://html.spec.whatwg.org/multipage/dom.html#kinds-of-content)
- [How to study HTML](https://fastcampus.co.kr/dev_red_jcm)
