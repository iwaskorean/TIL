# Webpack

웹팩이란 모듈을 위한 번들러이다. 모듈이란 import, export가 가능하며 재사용 가능한 코드 조각이며 번들링이란 js, css, asset 파일 등을 묶어주는 작업을 의미한다. 

웹팩은 모듈화 도구로서 다양한 로더를 추가할 수 있으며 여러 개의 파일을 하나로 번들링하기 때문에 HTTP 요청 횟수를 줄일 수 있다. 또한 다른리소스 포맷의 모듈도 사용할 수 있게 해준다

<br>

### Dependency Graph

한 파일이 다른 파일을 필요로하면 웹팩은 이것을 dependency로 처리하며 이를 통해 이미지나 웹 폰트와 같이 코드가 아닌 리소스를 dependency로 관리할 수 있게 한다. 

웹팩은 번들링 작업 시 dependency graph를 그리는데 dependency graph는 정의된 entry에서 출발해 어플리케이션에 필요한 모듈을 포함하는 그래프를 재귀적으로 빌드한다. 그래프를 완성시킨 후 하나 또는 소수의 번들로 번들링해 브라우저에 로드할 준비를 끝낸다.

<br>

## Core Concepts

다음은 웹팩을 이해하는데 필요한 핵심 개념들에 대한 설명이다.

<br>

### Entry

entry란 dependency graph의 출발점이다. entry를 통해 depency graph가 가장 먼저 포함할 모듈의 경로를 설정한다.

```javascript
// webpack.config.js

module.exports = {
  entry: './path/to/my/entry/file.js',
};
```

<br>

### Output

output은 번들링 완료된 파일을 내보낼 위치를 지정한다. 기본적으로 `dist` 디렉토리 하위 경로에 저장되며 파일 이름이나 경로를 따로 지정할 수도 있다.

`clean` 프로퍼티를 `true`로 설정하면 output 경로의 디렉토리에 사용하지 않는 파일을 자동으로 정리할 수 있다.

```javascript
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'webpack-practice.bundle.js',
  },
};
```

 <br>

### Loaders

웹팩은 기본적으로 자바스크립트와 json 파일만 이해할 수 있다. loader는 자바스크립트와 json 파일이 아닌 다른 유형의 리소스들을 dependency graph에 추가할 수 있는 모듈로 변환하는 역할을 한다.

v로더는 `test`와 `use` 프로퍼티를 가지는데, `test`에는 파일의 형식을, `use`에는 `test`에서 지정한 유형의 파일을 변환할 때 사용할 loader를 명시해야한다.

```javascript
// webpack.config.js

const path = require('path');

module.exports = {
  output: {
    filename: 'webpack-practice.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```

loader를 설정해주면 파일 유형에 상관없이 import가 가능하다.

<br>

### Plugins

loader는 특정 유형의 파일을 모듈로 변환하는 역할을 하지만 plugin은 번들 최적화, asset 관리 및 환경변수 설정 등에 사용된다.

plugin을 사용하기 위해서는 plugin 패키지를 import나 require해 가져온 다음 `plugins` 프로퍼티에 추가해야한다.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); 
const webpack = require('webpack'); 

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

<br>

## Setup for React app without CRA

###### ...cra 없이 리액트 셋업

<br>

<br>

------

**Reference**

- [webpack](https://webpack.js.org/concepts/)
- [Dependency Grah](https://webpack.js.org/concepts/dependency-graph/)
- [웹팩(Webpack) 밑바닥부터 설정하기](https://365kim.tistory.com/35)