# React webpack으로 빌드하기
- - -
 create-react-app을 사용해서 wepack이 갖추어져있는 환경을 사용할수도있지만
 간편함에 매몰되는 것 보다 어떤 원리로 내가 작성한 코드가 빌드되고 실행되는지를
 이해하보는 것이 당연히 더 좋다고 생각합니다.
 그래서 이번에는 webpack을 통해서 react를 빌드하는 초기셋업을 적어보겠습니다.
- - -

webpack 이란 ?
------
- - -
webpack을 쓰는 이유들은 여러가지이겠지만 가장 지금 시점에서 와닿는 이유는 
http로 js, css, html을 여러번 요청해야하는 리소스 낭비를 줄일 수 있다는 것 입니다.
개념적으로 한번에 모든 파일을 압축해서 보내게 되면 사용자의 입장에서 로딩이 길어지는
현상이 벌어질 수도 있는 문제점이 있지만, 내부적으로 청크와 같은 빌드 시스템을 도입해
이러한 문제들을 해결하려고 노력하고 있고 서비스를 무리없이 운영하기 위해 충분히
빠르기 때문에 매력적인 도구라고 생각됩니다.

순서
------
순서는 다음과 같이 진행하겠습니다.
 1. npm으로 필요한 라이브러리설치
 2. 단계별 webpack 적용 테스트
  - webpack.config.js / babel 등 설정파일에 대한 이해

- - -
 1. npm으로 필요한 라이브러리 설치
  - 설치할 사항이 꽤 많습니다. 다 이해하진 못했지만 단위 별로 주석을 달아보겠습니다.
 ```javascript
  npm i -D // 공통적으로 --save-dev로 설치했습니다.

  // 바벨관련 설치
  @babel/core 
  @babel/preset-env 
  @babel/preset-react 
  babel-loader 

  // html 및 css 로더 
  html-loader
  html-webpack-plugin
  css-loader 
  mini-css-extract-plugin 
  style-loader 
  // 사스를 사용할 경우에 여기선 사용하지 않지만 잊을까봐 적어둡니다.
  node-sass 
  sass-loader

  // React !
  react 
  react-dom  

  // webpack
  webpack 
  webpack-cli 
  webpack-dev-server
  clean-webpack-plugin // 쓰지 않는 파일을 자동적으로 정리해줍니다. 여기선 사용하지 않습니다.
```


2. 단계별 webpack 적용 테스트
 - 초기 빌드부터 html -> React -> css -> dev-server 순으로 어떻게 설정파일이 추가되는지
   알아보며 진행하겠습니다. webpack.config.js파일이 어떻게 변화되는지를 잘 추척해보면 이해가 
   빠르게 될 것 같습니다.
 - 완성된 폴더구조
  ├── package-lock.json
  ├── package.json
  ├── public
  │     └── index.html
  ├── src
  │   ├── App.js
  │   ├── index.js
  │   └── style.css
  └── webpack.config.js

  * 2-1 초기 빌드
    webpack.config.js를 생성해서 빌드가 잘 되었는지 보겠습니다.

    a. 먼저 프로젝트 최상단에 (root라고 부르겠습니다.) src 폴더를 생성하고 testBuild.js 파일을 생성합니다.
    ```javascript
    // testBuild.js
    console.log('이게 보이면 build가 성공한겁니다!')
    ```

    b. root에 webpack.config.js파일을 생성하고 다음과 같이 입력합니다.
    ```javascript
    // webpack.config.js
    const path = require("path");
    module.exports = {
      entry: "./src/testBuild.js", // 엔트리는 webpack으로 빌드할 첫번째 파일을 말합니다. 
      output: { // 엔트리로 시작한 빌드를 내보내는 설정입니다.
        filename: "bundle.js", // bundle.js라는 이름으로 빌드파일을 내보내겠다는 이야기 입니다.
        path: path.resolve(__dirname + "/build") // 내보낼 파일경로를 설정합니다. 생성이 안되어있다면 생성해줍니다.
      },
      mode: "none" // production 또는 develop 등 빌드시 모드를 적용해줍니다. none일때 defatult가 뭔지는 찾아봐야합니다.
    };
    ```

    c. package.json에 build script를 추가해줍니다.
    script부분에 아래와 같이 추가해줍니다.
    ```javascript
    // package.json
    "scripts": {
      "build": "webpack"
    },
    ```
    d. 테스트
    이제 terminal에 npm run build를 입력합니다.
    webpack.config.js에 path를 입력한데로 /build폴더안에 /bundle.js가 생성되면 성공입니다!

  * 2-2 html파일 빌드
    js파일이 성공적으로 빌드되었으면 다음은 html파일과 함께 빌드를 시켜보겠습니다.
    처음에 html관련 npm install이 html을 로드하기 위한 파일임을 이해할 수 있습니다.

    a. root에 public폴더를 생성하고 index.html파일 생성
    ```html
    // index.html
    <!DOCTYPE html>
      <head>
        <meta charset="utf-8" />
      </head>
      <body>
        <div id="root"></div>
      </body>
    </html>
    ```

    b. webpack.config.js파일에 html 설정 추가
    ```javascript
    // webpack.config.js
    const path = require("path");
    const HtmlWebPackPlugin = require("html-webpack-plugin"); // 플러그인을 설정합니다.
    module.exports = {
      entry: "./src/testBuild.js",
      output: {
        filename: "bundle.js",
        path: path.resolve(__dirname + "/build")
      },
      module: { // 앞으로 사용할 모듈들을 적는 곳 입니다. 
        rules: [ // 앞으로 loader들이 생성될 때 마다 이곳에 생성될 겁니다.
          {
            test: /\.html$/, // 빌드할 파일에 대한 정의입니다. 정규식으로 html파일을 가져온다는 것 입니다.
            use: [
              {
                loader: "html-loader",
              }
            ]
          }
        ]
      },
      plugins: [ // 이부분은 아직 잘 모르지만 이렇다를 알아두겠습니다 ㅜ 
        new HtmlWebPackPlugin({ // output에 있는 bundle.js를 자동으로 import해줍니다.
          template: './public/index.html',
          filename: 'index.html'
        })
      ]
    };
    ```

    d. 테스트
    다시 terminal에 npm run build를 입력합니다.
    build가 성공하고 index.html파일을 열어 콘솔창을 확인했을 때
    적어놓은 console.log('이게 보이면 build가 성공한겁니다!')가 잘 찍히면 성공입니다.

  * 2-3 React 빌드
    대망의 react 빌드를 시도해보겠습니다.

    a. src폴더에 index.js, App.js를 생성합니다.
  
    ```javascript
    // index.js
    import React from "react";
    import ReactDOM from "react-dom";
    import Root from "./Root";

    ReactDOM.render(<Root />, document.getElementById("root"));

    // App.js
    import React from 'react';

    const App = () => {
      return (
        <h1>Hello, React, Webpack!</h1>
      );
    };

    export default App;
    ```
    b. root폴더에 .babelrc를 추가합니다.
    import가 추가 된것을 확인하신 분이라면 babel이 왜 추가 되어야 하는지 이해하실 수 있을겁니다.
    import방식은 es6부터 지원이 되는 module 시스템이기 때문에 babel로 컴파일을 거쳐야 제대로 빌드할 수 있습니다.

    ```javascript
    // .babelrc
    {
    "presets": ["@babel/preset-env", "@babel/preset-react"]
    }
    ```

    c. webpack.config.js파일에 babel-loader 설정 추가
    바뀐 부분만 적고 있으니 주의하세요
    ```javascript
    // webpack.config.js
    const path = require("path");
    const HtmlWebPackPlugin = require("html-webpack-plugin"); // 플러그인을 설정합니다.
    module.exports = {
      entry: "./src/index.js", //이제 test를 벗어나서 빌드합니다.
      module: {
        rules: [ 
          { // 이곳이 추가되었습니다.
            test: /\.(js|jsx)$/,
            exclude: "/node_modules",
            use: ['babel-loader'], //설치한 loader를 사용합니다.
          },
          {
            test: /\.html$/,
            use: [
              {
                loader: "html-loader",
              }
            ]
          }
        ]
      }
    ```

    d. 테스트
    다시 terminal에 npm run build를 입력합니다.
    build가 성공하고 index.html파일을 열어 Hello, React, Webpack!가 보인다면 성공입니다. 

  * 2-4 css 및 webpack-dev-server 빌드
    글이 너무 길어져서 css와 dev-sever를 같이 적겠습니다. 
    css파일역시 html파일이나 여타 다른 파일을 빌드할 떄 추가하는 것 처럼 룰에 추가하는 형식이 되겠습니다.

    a. src폴더에 style.css 를 생성합니다.

    ```css
    style.js
    h1 {
      color: aquamarine;
    }
    ```

    b. webpack.config.js파일에 css / dev-server 설정 추가
    바뀐 부분만 적고 있으니 주의하세요
    ```javascript
    // webpack.config.js
    const path = require("path");
    const HtmlWebPackPlugin = require("html-webpack-plugin");
    const MiniCssExtractPlugin = require("mini-css-extract-plugin"); // plugin import
  
    module.exports = {
      module: {
        devServer: { // devserver를 추가합니다. 
          contentBase: path.resolve("./build"),
          index: "index.html",
          port: 3222 //서버가 구동되는 포트
        },
        rules: [
          {
            test: /\.html$/,
            use: [
              {
                loader: "html-loader",
              }
            ]
          },
          { // 여기가 추가 되었습니다.
            test: /\.css$/,
            use: [MiniCssExtractPlugin.loader, 'css-loader']
          }
        ]
        plugins: [
          new MiniCssExtractPlugin({
            filename: 'style.css'
          });
        ]
      }
    ```
    c. package.json에 dev-server script를 추가해줍니다.
    script부분에 아래와 같이 추가해줍니다.
    ```javascript
    // package.json
    "scripts": {
      "build": "webpack",
      "start": "webpack-dev-server --hot" //hot은 변경된 부분만 재 리로딩 하는 툴이라고 알고있습니다.
    },
    ```

    d. 테스트
    이번엔 terminal에 npm start 입력합니다.
    컴퍼알이 성공하면 localhost:3222로 접속해봅니다.
    접속이 성공하고 Hello, React, Webpack!의 색상이 변해있으면 css 성공입니다!
    또 react 코드를 아무거나 수정 후 저장하면 수정사항이 즉각반영됩니다.
  
* 참고
 아래 링크를 참조했습니다. 정말 많은 도움 되었습니다.
 저도 얼른 근본적인 이해들을 풀어낼 수 있는 수준의 글들을 작성해보고 싶네요!
 https://velog.io/@jeff0720/React-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD%EC%9D%84-%EA%B5%AC%EC%B6%95%ED%95%98%EB%A9%B4%EC%84%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Webpack-%EA%B8%B0%EC%B4%88