# Webpack



#### Webpack 

- JavaScript Application의 **Static Module Bundler**

- JavaScript 모듈화 도구로 다양한 로더를 추가할 수 있으며 트랜스파일 속도도 빠르다는 장점이 있다.

  > Module : Webpack을 구성하는 구성요소, js/stylesheet/image 등 모든 것이 모듈
  >
  > Bundler : 빌드에 필요한 파일들을 하나로 만들어서 전달하는 환경을 만듦



1. **Entry**

   - Webpack의 최초 진입점, 가장 먼저 읽을 파일의 경로 설정

   - Entry를 통해 모듈을 로딩하고 하나의 파일로 번들링

     ```
     // webpack.config.js
     module.exports = {
       entry: './src/app.js', 
       // 하나의 entry 파일 경로 설정
       
       entry: {
         app: ['./src/a.js', './src/b.js'],
     	// 다수의 entry 파일 경로 설정    
       },
     ```
   
   

2. **Output**

   - entry경로에 설정된 파일들을 하나로 묶은후 합쳐진 파일을 설정한 경로에 생성

     ```
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



3. **Loader**
   - Loader를 통해 다양한 리소스(스타일시트, 이미지 등)를 Webpack이 이해하고 처리 가능한 모듈로 변환



4. **Plugin**
   - 추가적인 기능 제공, 번들된 결과물을 처리(bundle optimization, asset management)

------

#### Install  

- Webpack 설치

  ```
  npm install -D webpack webpack-cli
  ```

  - global이 아닌 local로 설치 권장

- Babel

  ```
  npm install -D @babel/core @babel/preset-env babel-loader 
  ```

  > Babel은 Javascript ES6 문법을 ES5로 변환해주는 트랜스파일러

- Loader and Plugin

  ```
  npm install -D browser-sync browser-sync-webpack-plugin html-webpack-plugin  css-loader sass-loader style-loader file-loader clean-webpack-plugin
  ```

  - css/sass/style-loader : 스타일시트 파일 번들링
  - filer-loader : 이미지파일(png, img 등)을 번들링
  - clean-webpack-plugin : 빌드될때마다 output 경로의 dir 초기화
  - browser-sync, browser-sync-webpack-plugin : 브라우저 자동 동기화
  - html-webpack-plugin : html 파일의 번들링을 위한 Plugin

------

#### Configuration

1. Loader 설정

   ```
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
         },
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

   ```
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

   - production : 배포용, 최적화 빌드 
   - development : 개발용, 빠르게 빌드
   - none 

   ```
   // webpack.config.js
   module.exports = {
   	...
     mode: 'development',
     	...
   }
   ```

4. package.json

   ```
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

   



------

**Reference**

https://webpack.js.org/

[medium](https://medium.com/wasd/%EC%9B%B9%ED%8C%A9-webpack-%EA%B3%BC-%EB%B0%94%EB%B2%A8-babel-%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-react-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-fb87d0027766)

[velog](https://velog.io/@hih0327/Webpack-%EA%B8%B0%EC%B4%88)