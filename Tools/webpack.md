# Webpack

웹팩이란 자바스크립트 어플리케이션의 js파일, 스타일시트, 이미지 등의 모듈들을 하나의 파일로 묶는 정적 모듈 번들러(static module bundler)이다. 또한 모듈화 도구로서 다양한 로더를 추가할 수 있고 트랜스파일 속도가 빠르다는 장점이 있다.

<br>

### Entry

entry란 웹팩의 최초 진입점이며 가장 먼저 읽을 파일의 경로를 설정한다. entry를 통해 모듈을 로딩하고 하나의 파일로 번들링한다.

```javascript
// webpack.config.js
module.exports = {
  entry: './src/app.js', 
  // 하나의 entry 파일 경로 설정
  
  entry: {
    app: ['./src/a.js', './src/b.js'],
	// 다수의 entry 파일 경로 설정    
  },
```

<br>

### Output

entry 경로에 설정된 파일들을 하나로 묶은 후 합쳐진 파일을 설정한 경로에 생성한다.

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
	...
  output: {
    path: __dirname + '/dist/',
    filename: 'bundle.js',
    // 합쳐진 파일은 dist/bundle.js로 생성
  },
 }
```

<br>

### Loader

loader를 통해 스타일시트, 이미지 등의 리소스를 웹팩이 이해하고 처리 가능한 모듈로 변환하는 역할을 한다.

<br>

### Plugin

pulgin은 추가적인 기능들을 제공하며 번들 최적화(bundle optimization), 리소스 관리(asset management) 등 번들된 결과물들을 처리하는 역할을 한다.

<br>

<br>

## Install  

- 웹팩 설치

  ```
  npm install -D webpack webpack-cli
  ```

  - global이 아닌 local로 설치 권장

- Babel

  bable은 자바스크립트 ES6 문법을 ES5로 변환해주는 트랜스파일러이다.

  ```
  npm install -D @babel/core @babel/preset-env babel-loader 
  ```

- Loader and Plugin

  ```
  npm install -D browser-sync browser-sync-webpack-plugin html-webpack-plugin  css-loader sass-loader style-loader file-loader clean-webpack-plugin
  ```

  - css/sass/style-loader : 스타일시트 파일 번들링
  - filer-loader : 이미지파일(png, img 등)을 번들링
  - clean-webpack-plugin : 빌드될때마다 output 경로의 dir 초기화
  - browser-sync, browser-sync-webpack-plugin : 브라우저 자동 동기화
  - html-webpack-plugin : html 파일의 번들링을 위한 Plugin

<br>

<br>

## Configuration

1. Loader 설정

   ```javascript
   // webpack.config.js
   module.exports = {
   
   	...
   	
   	module: {
       rules: [
         {
           test: /\.m?js&/,
           exclude: /(node_modules|bower_components)/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@bable/preset-env'],
             },
           },
         }
         {
           test: /\.(sc|sa|c)ss$/,
           use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'],
         },
         {
           test: /\.(png|svg|jpg|gif)$/,
           use: [
             {
               loader: 'file-loader',
               options: {
                 name: '[name].[ext]?[hash]',
               },
             },
           ],
         },
       ],
     },
     
     ...
   
   }
   ```

   

2. Plugin 설정

   ```javascript
   // webpack.config.js
   const BrowserSyncPlugin = require('browser-sync-webpack-plugin');
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   const MiniCssExtractPlugin = require('mini-css-extract-plugin');
   const { CleanWebpackPlugin } = require('clean-webpack-plugin');
   
   module.exports = {
   	...
   	
     plugins: [
       new HtmlWebpackPlugin({
         filename: 'index.html',
         favicon: './favicon.png',
         template: 'src/index.html',
       }),
       new CleanWebpackPlugin({
         cleanAfterEveryBuildPatterns: ['dist'],
       }),
       new MiniCssExtractPlugin({
         filename: 'style.css',
       }),
       new BrowserSyncPlugin({
         host: 'localhost',
         post: 3000,
         server: { baseDir: ['dist'] },
         files: ['./dist/*'],
         notify: false,
       }),
       
       ...
     ],
   ```

3. Mode

   웹팩에서 제공하는 3가지 모드는 아래와 같다.

   - production : 배포용, 최적화 빌드 
   - development : 개발용, 빠르게 빌드
   - none 

   ```javascript
   // webpack.config.js
   module.exports = {
   	...
     mode: 'development',
     	...
   }
   ```

4. package.json

   ```json
   //package.json
   {
   	...
     "main": "webpack.config.js",
     	...
     "scripts": {
       "dev": "webpack --config webpack.config.js",
       "start": "webpack --config webpack.config.js"
     },
     	...
   }
   ```


<br>

<br>

------

**Reference**

- [webpack documentation](https://webpack.js.org/concepts/)
- [Webpack과 Babel을 이용한 React 개발 환경 구성하기](https://berkbach.com/%EC%9B%B9%ED%8C%A9-webpack-%EA%B3%BC-%EB%B0%94%EB%B2%A8-babel-%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-react-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-fb87d0027766)
- [Webpack 기초](https://velog.io/@hih0327/Webpack-%EA%B8%B0%EC%B4%88)