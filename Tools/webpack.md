# Webpack

웹팩이란 모듈을 위한 번들러이다. 모듈이란 import, export가 가능하며 재사용 가능한 코드 조각이며 번들링이란 js, css, asset 파일 등을 하나의 파일로 묶어주는 작업을 의미한다. 

웹팩은 모듈화 도구로서 다양한 로더를 추가할 수 있으며 여러 개의 파일을 하나로 번들링하기 때문에 HTTP 요청 횟수를 줄일 수 있다. 또한 다른 리소스 포맷의 모듈도 사용할 수 있게 해준다

<br>

### Dependency Graph

한 파일이 다른 파일을 필요로하면 웹팩은 이것을 dependency로 처리하며 이를 통해 이미지나 웹 폰트와 같이 코드가 아닌 리소스를 dependency로 관리할 수 있게 한다. 

웹팩은 번들링 작업 시 dependency graph를 그리는데 dependency graph는 정의된 entry에서 출발해 어플리케이션에 필요한 모듈을 포함하는 그래프를 재귀적으로 빌드한다. 그래프를 완성시킨 후 하나 또는 소수의 번들로 번들링해 브라우저에 로드할 준비를 끝낸다.

<br>

## Core Concepts

![](https://nesoy.github.io/assets/logo/webpack.png)

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

<br>

## Setup for React app without CRA

다음은 create react app(cra) 없이 웹팩을 구성하는 예제이다. 

### Why it is better to avoid Create React App(CRA)

cra는 웹팩과 웹팩 구성 요소들을 알지 못하더라 리액트를 사용할 수 있게 리액트 개발 환경을 구축해주는 도구이다. 그러나 cra는 웹팩 설정을 제한하고 불필요한 dependency들이 많으며 환경 설정을 사용자가 직접 정의하지 못한다는 단점이 있다.

<br>

### Install Webpack

가장 먼저 웹팩 패키지를 설치해야한다.

```
npm install webpack
	or
yarn add webpack
```

### Configure Webpack

다음으로 웹팩 config 파일인 `webpack.config.js` 파일을 생성한다.

### Add Development mode

`webpack.config.js`파일에서 `mode`를 development로 설정한다.

```javascript
// webpack.config.js

module.exports = {
  mode: 'development',
};
```

```
yarn add --dev webpack-cli
```

웹팩 cli 패키지를 설치한 후 빌드하면 `dist` 디렉토리에 다음과 같은 파일이 실행된다.

```javascript
// src/index.js

const test = 'test';
```

```javascript
// dist/main.js

(() => {
  var __webpack_modules__ = {
    './src/index.js': () => {
      eval(
        "const test = 'test';\r\n\n\n//# sourceURL=webpack://webpack-practice/./src/index.js?"
      );
    },
  };


  var __webpack_exports__ = {};
  __webpack_modules__['./src/index.js']();
})();

```

웹팩은 모듈 번들리이다. 다른 설정이 없으면 `src` 디렉토리에 생성한 `index.js`를 사용하고 모든 dependency들을 하나의 파일 즉, `main.js`로 번들링한다.

### Add the Webpack server

다음은 저장 및 새로고침 없이 브라우저에서 코드 변경을 확인하기 위해 webpack-server를 설치하는 과정이다.

```
yarn add --dev webpack-server webpack-dev-server
```

위와 같이 패키지 설치 후 `package.json` 스크립트에 webpack serve 명령을 추가한다

```json
// package.json

"scripts": {
  "start": "webpack serve"
},
```

이제 yarn을 통해 사이트를 컴파일 및 serve 할 수 있다.

### Add HTML Webpack Plugin

`index.html`을 생성하고 번들된 js 파일을 추가하기 위해  html-webpack-plugin을 설치해야한다. 

```
yarn add html-webpack-plugin
```

설치 후 해당 plugin을 가져와 웹팩 설정 파일에 추가해준다.

```javascript
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin');

const htmlWebpackPlugin = requrie('html-webpack-plugin');

module.exports = {
  mode: 'development',
  devServer: {
    port: 8001,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
};
```

이제 `template` 프로퍼티에 지정된 경로에 위치한 html 파일을 템플릿으로 사용할 수 있다.

`yarn start` 커맨드로 빌드 후 `localhost:8001`에서 개발자 도구를 통해 source를 확인하면 `index.html`파일에서 script 태그를 설정하지 않았지만 플러그인에 의해 추가되어있음을 확인할 수 있다.

![](https://miro.medium.com/max/1050/1*8q6cG3eGTWNY9aEYYO4mpA.png)

### Link JavaScript with HTML

다음은 html 파일을 통해 브라우저에 js 파일의 내용을 표시해야한다. 

먼저 `index.html` 파일을 다음과 같이 수정한다.

```html
// index.html

...
  <body>
    <h1>This is a html file.</h1>
    <div id="fromjs"></div>
  </body>
...
```

`index.js` 파일을 다음과 같이 수정한다.

```javascript
document.querySelector('#fromjs').innerHTML = `<h1>From index.js</h1>`;
```

다시 yarn을 통해 빌드하면 "This is a html file." 아래에 js 파일을 통해 추가한 "From index.js" 내용이 출력된다.

<br>

### Add React to Webpack

다음과 같이 리액트 패키지를 설치한다.

```
yarn add react react-dom --save
```

설치 후 `index.js` 파일에 다음과 같은 코드를 추가한다.

```javascript
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render('Hello from index.js', document.querySelector('#fromjs'));
```

이제 터미널에서 `yarn start` 커맨드를 입력하면 `ReactDOM`을 통해 렌더한 컨텐츠가 출력된다.

그러나 'Hello from index.js'가 아닌 `<h1>Hello from index.js</h1>`를 입력할 경우 컴파일 되지 않는다.

이때 필요한 것이 바로 babel이다.

### Add Babel to Webpack

트랜스파일이란 다른 언어로 변환시키는 작업을 의미한다. babel은 ES6 문법으로 작성된 코드를 ES5 문법으로 트랜스파일해주는 트랜스파일러이며 babel-loader는 babel과 웹팩을 연결시켜주는 loader이다.

다음과 같이 패키지를 설치한다.

```
yarn add @babel/core @babel/plugin-transform-runtime @babel/preset-env @babel/preset-react babel-loader 
```

패키지 설치 후 `webpack.config.js` 파일의 `module` 프로퍼티를 다음과 같이 수정한다.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  devServer: {
    port: 8001,
  },
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-react', '@babel/preset-env'],
            plugins: ['@babel/plugin-transform-runtime'],
          },
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
};

```

babel 설치 및 babel-loader를 웹팩과 연결하는 설정을 완료한 후에 `index.js` 파일을 다음과 같이 수정한다.

```javascript
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<h1>Hello from js</h1>, document.querySelector('#fromjs'));
```

### Add CSS to Webpack

다음은 CSS를 사용하기 위해 웹팩에 추가하는 예제이다.

먼저 css-loader와 style-loader 패키지를 설치한다.

```
yarn add css-loader style-loader
```

`webpack.config.js` 파일의 `moduel` 프로퍼티를 다음과 같이 수정한다.

```javascript
module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-react', '@babel/preset-env'],
            plugins: ['@babel/plugin-transform-runtime'],
          },
        },
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
```

이제 `src` 하위 경로에 css파일을 추가하고 `index.js`에서 css 파일을 import하면 프로젝트에 css 스타일을 적용할 수 있다.

```css
// src/styles/style.css

* {
  color: red;
}
```

```javascript
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './styles/styles.css';

ReactDOM.render(<h1>Hello from js</h1>, document.querySelector('#fromjs'));
```

<br>

<br>

------

**Reference**

- [webpack](https://webpack.js.org/concepts/)
- [Dependency Grah](https://webpack.js.org/concepts/dependency-graph/)
- [웹팩(Webpack) 밑바닥부터 설정하기](https://365kim.tistory.com/35)
- [Freedom from create-react-app (How to Create React Apps without CRA)](https://levelup.gitconnected.com/freedom-from-create-react-app-how-to-create-react-apps-without-cra-27fadeb79c82)
- [Adding React to Webpack](https://www.linkedin.com/pulse/adding-react-webpack-rany-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0)
- [Adding Babel to Webpack](https://www.linkedin.com/pulse/adding-babel-webpack-rany-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0)
- [Adding CSS to Webpack](https://www.linkedin.com/pulse/adding-css-webpack-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0)