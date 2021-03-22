# Pseudo Classes vs Pseudo Elements



### Pseudo Class

- 일반적인 선택자로 표현 할 수 없는 것을 선택

- 형제 / 자식 요소의 상대적인 위치, 특정한 상태 등 일부 조건에서 선택

- ##### Dynamic pseudo-classes

  - `:link`, `:visited`, `:hover`, `:active`, `:focus`

- ##### Structural pseudo-classes

  - `:first-child`, `:nth-child(n)`, `:nth-last-child(n)`, `:nth-of-type(n)`, `:last-child`, `:root`, `:first-of-type`,  ...

- ##### UI element states pseudo-classes

  - `:enabled`, `:disabled`, `:checked`

- ##### Other pseudo-classes

  - `:not(x)`, `:target`, `:lang(language)`





### Pseudo Element

- 문서 마크업에 지정되지 않은(문서 트리에 존재하지 않는) 요소를 효과적으로 생성
- `::before`, `::after`, `::first-letter`, `::first-line`





### :: before vs : before

- `:before`는 IE8 이하에서 지원, `::before`는 CSS3부터 도입
- Which one is correct?
  - CSS3 이전에 개발된 많은 사이트들은 `:before`를 이용해 만들어졌고 브라우저간의 호환성을 고려했을때 `:before`를 사용해야 하는 이유도 존재하지만 최신 문법인  `::before`의 사용 지향








------

**Reference**

- [Pseudo-classes vs pseudo-elements](https://www.growingwiththeweb.com/2012/08/pseudo-classes-vs-pseudo-elements.html)

- [::before vs :before](https://css-tricks.com/to-double-colon-or-not-do-double-colon/)

  