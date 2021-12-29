# 마크업 가이드

HTML은 웹 페이지의 레이아웃을 구성하는 마크업 언어이자 웹 페이지 내의 요소가 어떤 역할을 하는지 의미를 전달하는 수단이다.

의미론적으로 올바르게 마크업을 작성하는 방법에 대해 알아보자.

<br>

## 개요 알고리즘의 이해

웹 문서의 개요는 추려낸 주요 내용에 대한 윤곽이다. 개요는 헤딩과 섹션으로 형성된다.

### TITLE vs HEADING

title은 문서 내 한번만 사용해야 한다. heading 요소는 한 페이지에 두 번 이상 사용할 수 있다.

### HEADING

**heading 없이는 개요도 없다.**

- heading은 문서 개요를 형성하는 필수 요소이다.
- 웹 브라우저와 화면낭독기에 문서 개요를 드러내는 방법이다.
- heading 요소간에 위계 관계가 있기 떄문에 h1부터 순차적으로 작성하는 것을 지향해야 한다.

```
// HTML
<h1> A
<h2> B
<h3> C

// 브라우저
A
  B
    C
```

### SECTIONING CONTENT

섹셔닝 콘텐츠는 HTML5에서 추가된 개요의 범위를 명시적으로 지정하는 개요 컨테이너다.

적절한 수준의 heading을 자식 요소로 사용해야 한다.

- article: 기사, 본문 등 맥락에 독립적으로 배포 가능
- aside: 페이지의 주요 내용이 아닌 것들
- nav: 사이트의 주된 탐색 메뉴
- section: 주제별로 나누거나 묶을 수 있는 것들

```
// 잘못된 섹셔닝 콘텐츠 마크업
<h1> A
  <article>
    <h1> B
      <section>
        <h1> C

// 브라우저
A
B
C
```

```
// 올바른 섹셔닝 콘텐츠 마크업
<h1> A
  <article>
    <h2> B
      <section>
        <h3> C

// 브라우저
A
  B
    C
```

### 올바른 섹션

명시적 섹션이란 섹셔닝 콘텐츠를 헤딩과 함께 사용해 섹션의 범위를 명시적으로 알 수 있는 섹션이다. 섹셔닝 콘텐츠를 사용하지 않고 헤딩만을 사용하면 암시적으로 섹션이 생성된다.

올바른 마크업을 위해서는 명시적 섹션을 사용해야 한다.

```
// 명시적 섹션(Good)
<h1> A
  <article>
    <h2> B
      <section>
        <h3> C

// 암시적 섹션(Bad)
<h1> A
<h2> B
<h3> C
```

또한 섹션이 있는데 섹션 내부에 헤딩 요소가 없으면 문법적으로 잘못된 마크업은 아니지만 매우 어색한 섹션이 된다.

**섹셔닝 요소를 적극적으로 사용하되 브라우저에서 헤딩 수준을 자동으로 보정해주는 아웃라인 알고리즘 명세에 의존해서는 안된다.**

<br>

## HTML Sementics

의미론적인 HTML이란 웹 페이지를 작성한 의도를 사용자에게 정확하게, 다채롭게 전달하기 위한 HTML 작성 방법이다.

### div, span

div와 span 요소는 아무런 의미가 없는 요소이다.

- div와 span 요소를 많이 사용한다는 것은 HTMl 요소를 의미적절하게 사용하고 있지 않다라는 것이다.
- 담고 있는 내용의 의미를 찾지 못했을 때 사용한다.
- 항상 마지막 선택지로 생각해야 한다.

```
// div를 대체할 수 있는 요소들
h1, h2, h3, h4, h5, h6, p, ul, ol, li, dl, dt, dd, blockquote, pre, address

article, aside, nav, section, hgroup, header, footer,
main, figure, figcaption, details, summary, dialog, datalist
```

```
// span 요소를 대체할 수 있는 요소들
a, em/strong, label, q, sub/sup, ins/del, code, dfn, abbr, cite, kbd, ruby, samp, var, small, b, u, i, s

data, time, mark, output, meter, progress
```

**`<div>`, `<span>` 요소는 콘텐츠를 의미 단위로 그룹핑하는 요소가 아닌 CSS나 자바스크립트로 DOM을 조작할 필요가 있을 때 편의상 사용하는 요소다.**

### SECTIONING

섹셔닝 요소에는 article, aside, nav, section이 있다.

- section: 제목이 있는 주제별 콘텐츠 그룹 또는 영역이다.
- article: 기사, 본문, 댓글 등 섹션 요소 중 독립적으로 배포 가능한 콘텐츠다.

섹션과 아티클은 h1 ~ h6 요소를 포함하도록 작성하는 것이 좋다.

```HTML
<!-- 주제별 section 활용 -->
<section>
  <h2> 섹션 헤딩 </h2>
</section>

<!-- 독립적 배포가 가능한 완결형 article 활용 -->
<article>
  <h2>완결형, 독립적 콘텐츠 헤딩</h2>
</article>
```

섹션 내부에 또다른 섹션, 아티클 내부에 또다른 아티클을 작성할 수 있다.

### header, footer

header와 footer는 섹셔닝 요소는 아니지만 섹셔닝 요소와 함께 또는 섹션이 요소 내부에서 사용할 수 있는 요소다.

- header: 도입부, 헤딩, 목차, 검색, 로고 등에 사용된다.
- footer: 저자, 저작권, 연락처, 관련 문서 등

필수적인 요소는 아니지만 위와 같은 내용의 콘텐츠들을 담을 때 div 대신 사용할 수 있다.

### nav

nav 요소는 현재 사이트나 현재 페이지 일부를 링크하고 있는 주요 탐색 섹션이다.

- 메뉴나 목차
- footer의 약관, 저작권 고지 링크 등 페이지의 주요한 내용 링크가 아닌 내용은 nav에 들어가기 적절하지 않다.
- h1 ~ h6 요소를 포함해 작성하는 것이 권장된다.

### aside

aside는 페이지의 주 내용과 관련성이 부족해 구분할 필요가 있는 섹션이다.

- 부수적인 콘텐츠, 배너 영역, 관심있을 만한 콘텐츠 등의 부수적인 섹션이다.
- 부수적인 콘텐츠, 관련 기사, 광고 등 내용이 포함된다.
- h1 ~ h6 요소를 포함해 작성하는 것이 권장된다.

### main

main은 문서의 핵심 주제 또는 어플리케이션의 핵심 기능과 직접 관련된 콘텐츠 영역을 의미한다.

- 섹셔닝 콘텐츠가 아니다. 즉 헤딩이 필요하지 않다.
- 페이지마다 반복되지 않는 내용, 고유한 콘텐츠를 포함해야 한다.
- 하나의 페이지에 다수의 main 요소가 존재할 수 있지만 하나의 main만 표시되어야 한다.
- body, div 요소를 제외한 다른 요소의 자식이 될 수 없다.
- main 내부에 article, section 등이 포함된다.

### dialog

dialog는 사용자와 상호 작용하는 대화 상자 콘텐츠이다.

- open 어트리뷰트를 추가해 활성화할 수 있다.
- 키보드 포커스가 dialog 안쪽으로 들어와야 한다.
- tab 키만을 이용해 dialog 안쪽에 이동가능한 컨트롤들을 탐색할 수 있어야 한다.
- 포커스가 dialog 밖으로 빠지면 안된다.

### 기타 요소들

- mark: 사용자의 주의를 끌기 위한 하이라이트 요소이다.
- figure, figcaption: 문서의 주된 흐름을 위해 참조하는 완결형 요소이며 도표, 이미지, 코드등의 내용물과 설명을 포함한다.
- ins, del: 추가하고 삭제한 내용을 의미한다.
- progress: 진척도를 나타낸다. 텍스트 노드 작성을 통해 구형 브라우저에서 인식하지 못할 때 대체할 수 있다.
- b, i, s, u: strong, em, dem ins 요소로 대체할 수 있다.

<br>

## Interactive Content

인터렉티브 콘텐츠는 상호자와 상호 작용할 수 있는 콘텐츠이다. 입력장치로 조작할 수 있다.

```
a, audio, button, details, embed, iframe, img, input, label,
select, textarea, video

display: inline | inline-block;
```

### `<a>` vs `<button>`

같은 외형이라도 a와 button 요소는 용도에 따라 구분해서 사용해야 한다.

- a
  - 실행 결과를 가리킬 수 URL이 있다.
  - 링크위에 마우스를 올리면 브라우저 상태 표시줄에 타겟 URL이 표시된다.
  -
- button
  - 참조할 URL이 없을 때 즉, 타겟 URL을 설정할 수 없을 때 사용해야 한다.
  - URL이 아니라 콘텐츠들이 바뀔 때 사용한다.

CSS 명세의 `cursor: pointer`는 요소가 링크와 연결되어 있을 때 사용하라고 명시되어 있다. 따라서 button 요소에는 손 모양을 사용하지 않는 것이 적절하다.

### `<a target>`

a의 target 속성을 `_blank`로 설정하면 새로운 탭에서 해당 링크 사이트가 열리게 된다. 이것은 자식 창에서 부모 창의 권한을 획득할 수 있는 탭 가로채기 공격에 대상이 될 수 있다.

rel 속성을 통해 이를 방지할 수 있다.

```html
<a href="https://example.com" target="_blank" rel="noopner noreferrer"></a>
```

rel 속성에 nooper를 추가하면 window.opener 객체를 제거한다. noreferrer는 이 객체를 제어할 수 없게 만들며 올드 브라우저를 위해 함께 noreferrer를 같이 표기하는 것이 좋다. 최신 브라우저는 nooper 값을 암시적으로 적용한다.

### `<details>`와 `<summary>`

details 요소는 열림 상태일 때 정보를 표시하는 요소이다. open 속성을 통해 컨트롤할 수 있다.

summary 요소는 details 요소의 자식 요소이며 나머지 부분에 대한 요약, 캡션, 범례를 의미한다.

```html
<details open>
  <summary>마크업 가이드</summary>
  <p>의미 적절한 html 요소를 사용하자.</p>
</details>
```

### `<input type>`

다양한 쓰임의 type 속성을 알면 적절하게 input을 활용할 수 있다.

다음은 type 속성 값에 대한 input 요소의 설명이다.

- search: 검색창, X 버튼이 함께 표시된다.
- tel: 모바일 기기에서 전화번호 키패드가 제공된다.
- url: 키패드에 슬래시와 .com이 함께 제공된다.
- email: 사용자가 과거에 사용한 이메일 자동완성 기능이 제공된다.
- date: 날짜나 시간에 대한 포맷이 제공된다.
- month/week/year: 월/주/년 포맷이 제공된다.
- color: 컬러 피커가 제공된다.

### `<input attr>`

input 요소의 기타 속성들에 대한 설명이다.

- required: 사용자가 input에 값을 작성하지 않거나 조건에 맞지 않을 때 에러 메시지나 도움말 메시지를 표시한다. 브라우저에 내장되어 있기 때문에 스타일 커스터마이징이 불가하다.
- placeholder: 데이터 입력을 위한 샘플이나 힌트를 제공한다. label 대신 placeholder를 사용하는 것은 사용자 경험을 떨어트릴 수 있으므로 label을 꼭 같이 써주는것이 좋다.

### <datalist>

datalist는 다른 컨트롤을 위해 미리 정의된 옵션 세트를 의미한다. 보기, 연관 검색어와 같이 미리 정의된 옵션을 생각하면 된다.

```html
<label for="local">지역번호</label>
<input type="text" id="local" value="?" list="local-list" />

<datalist id="local-list">
  <option value="02" label="서울"></option>
  <option value="051" label="부산"></option>
</datalist>
```

<br>

## 이미지 마크업

대용량 이미지는 사용자 경험을 떨어트리는 요소이다. 여러 가지 포맷을 제공한 다음 사용자에게 알맞는 포맷의 이미지를 표시하도록 마크업해야 한다.

- jpg/png: 높은 압축률로 화질이 깨질 수 있다. 올드 브라우저 호환, 폴백 이미지 수단으로만 사용하는 것이 좋다.
- webp: jpg 대비 30 ~ 70% 수준의 용량이다.
- avif: 저용량과 고품질 수준의 포맷이다.

### `<picture>`, `<source>`, `<img>`

picture 요소의 type 속성을 통해 사용자에게 맞는 포맷에 대한 조건 분기가 가능하다.

```html
<picture>
  <source srcset="ex.avif" type="image/avif" />
  <source srcset="ex.webp" type="image/webp" />
  <img src="ex.png" alt />
</picture>
```

다음과 같이 마크 업하면 사용자가 avif 포맷을 사용할 수 있는 환경이면 avif를 그렇지 않을 경우 webp 포맷의 이미지를 표시하며 두 포맷 다 지원하지 않을 경우 png 포맷의 이미지를 제공한다.

media 속성과 resolution 속성을 통한 분기도 가능하다.

```html
<picture>
  <!-- 해상도 구간, 사용자의 디바이스 width에 맞추어 이미지를 제공한다. -->
  <source srcset="large.webp" media="(max-width:960px)" />
  <img src="small.webp" alt />
</picture>

<picture>
  <!-- 해상력을 고려해 이미지를 제공한다. img 요소의 srcset에도 적용할 수 있다. -->
  <source srcset="2ximage.webp 2x, 1ximage.webp" type="image/webp" />
  <img srcset="2x.jpg" src="1x.jpg" alt />
</picture>
```

picture와 source 요소는 화면에 표시되는 요소가 아니라 어떤 img 요소를 화면에 표시할 것인지를 정하는 요소이다.

---

### Reference

- [견고한 UI 설계를 위한 마크업 가이드](https://fastcampus.co.kr/dev_red_jcm)
