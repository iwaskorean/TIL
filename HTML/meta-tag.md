# meta tag

`<meta>` 태그는 HTML 문서의 메타데이터를 정의하는 태그이다. 메타데이터란 데이터에 대한 데이터 즉, 다른 데이터를 설명하는 데이터이다. 

 `<meta>` 태그는 항상 `<head>` 요소 내부에 위치해야하며 character set, 페이지 설명, 키워드, 페이지 작성자, viewport 설정 등의 메타데이터의 정의를 정의한다. 이렇게 정의된 HTML 문서의 메타데이터들은 브라우저, 검색 엔진 등의 웹 서비스에서 사용된다.

<br>

### Attribute

`<meta>` 태그는 다음과 같은 속성을 가진다.

- charset : HTML 문서의 문자 인코딩 정보를 지정한다.
- content : http-equivor 또는 name 속성과 관련된 값을 지정한다.
- http-equiv : content 속성의 정보와 값 HTTML 헤더를 제공한다.
- name: 메타데이터의 이름을 지정한다.

<br>

### Example

**문자인코딩 지정**

```javascript
<meta charset="utf-8" />
```

**검색 엔진에 대한 키워드 정의**

```javascript
<meta name="keywords" content="HTML, CSS, JavaScript" />
```

**웹 페이지의 설명 정의**

```javascript
<meta name="description" content="This is a description of web page." />
```

**페이지의 저자 정의**

```javascript
<meta name="author" content="Sewook Han" />
```

**http-equiv 속성을 이용해 Content-Type 지정**

```javascript
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

**http-equiv 속성을 이용해 30초마다 새로고침 설정**

```javascript
<meta http-equiv="refresh" content="30" />
```

**viewport 설정**

```javascript
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

viewport란 사용자들이 볼 수 있는 웹 페이지 영역이다. 스마트폰 화면이 컴퓨터 화면보다 작듯이 디바이스마다 viewport가 다르다.

`viewport` 메타데이터의 `content` 속성은 디바이스 스크린 폭에 따라 페이지 폭을 설정하는  `width=device-width`와 페이지가 브라우저에 의해 처음 로드될 때 줌 레벨을 설정하는 `initial-scale=1.0`가 있다.

 <br>

### Search Engine Crawlers with meta tags

검색 엔진 크롤러는 `<meta>` 태그로 다음과 같은 작업들을 수행한다.

1. 인덱싱(indexing) : 인덱싱이란 검색 엔진이 페이지를 크롤링할 때 HTML 코드의 복사본을 데이터베이스의 저장하는 작업이다. 이 작업에서 구글은 모든 메타 키워드들에 대한 색인을 생성하고 키워드들을 인식한 후 저장한다.
2. 검색(retrieval) : 검색 엔진이 웹 사이트에 대해 인덱싱한 내용을 검색한 후 사용자의 검색어와 비교한다. 
3. 랭킹(ranking) : 가장 관련성 있는 웹 사이트를 가장 최상위에 순위로 랭킹한다.

<br>

<br>

------

**Reference**

- [HTML <meta> Tag](https://www.w3schools.com/tags/tag_meta.asp)
- [What Search Engine Crawlers do with your Meta Tags](https://seo-hacker.com/what-search-engine-crawlers-do-with-your-meta-tags/)