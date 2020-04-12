# webpack
- - -
webpack 설정은 굉장히 어렵다. 
물론, 하나의 모듈로 생각한다는 전제 자체가 이해가 잘 안되기 떄문이다.
여전히 헷갈리지만 다음에 적용을 쉽게하기 위해 헷갈렸던 개념들과 적용법을 기록한다.
- - -

1. webpack은 Front end 영역이 전문이다.

 - 이걸 몰라서 적용방법에 한참을 고민했다. web영역에 여러 파일들을 불러오는 비효율성을 해결하고자 나오는 개념이라고 알고있음에도 불구하고 
 - 쉽사리 체감이 되지 않았던 것 같다. 
 - node.js로 구성된 BackEnd를 사용하려면 webpack.config.js에 관련 설정을 추가해야한다.
 - taget 을 지정해줘야하는데 설정을 하지 않으면 default는 web이다.

```javascript
    const nodeExternals = require('webpack-node-externals');
    target: 'node',
    externals: [nodeExternals()]
```

2. 내가 작성한 파일들을 번들링해야할 경우?

 - externals 라는 개념을 사용해야한다.
 - 아직 정확하게 파악은 안되었는데 redis를 따로 추가 하던가
 - 외무 모듈들과의 의존성을 끊은 것들을 번들링해야할 경우 사용해야한다고 한다.
 ```javascript
    const nodeExternals = require('webpack-node-externals');
    externals: [nodeExternals()]
```

3. loader

 - 굉장히 헷갈렸는데 개발하고있는 폴더에 모든 것들을 loader에 추가하는 것이 아니라
 - 번들링 되는 파일안에 js파일이 아닌경우 loader에 추가해나가는 개념이다. 
 - 어떻게 보면 당연한건데 굉장히 헷갈렸다.
 - webpack 4 기준으로 사용법을 보면 이렇다.

```javascript
  {
    test: 'html' // 어떤 형식을 만날떄
    use: 'html-loader', // 이걸 사용한다.
  }
```

4. htmlWebpackPlugin

 - 이것도 잘못 사용하고 있는 것 중에 하나였다. 
 - 이 것을 사용하면 html 파일에 번들링하는 js 파일을 자동으로 추가해준다. (이게 용도인지는 모르겠지만)
 - 예를들어 아래처럼 html 파일에 scipt를 추가하면서 개발했던 것을

```html
  <button>click</button>
  <script src='./onBtnClick.js'> </script>
```

 - htmlWebpackPlugin 사용한다면 html파일에 script를 하드코딩하지 않고
 - webpack.config.js에 다음과 같이 설정해서 자동으로 추가시킨다.
 - 예를들어 index.html에 index.js를 추가하고 싶으면 excludeChunks에 index를 추가한다.
 - chunks를 사용하면 chunks에 정의된 이름 이외의 entry 파일이 추가된다.
 - 아무것도 지정하지 않으면 entry에서 번들된 모든 파일이 추가된다. (이 부분을 몰라서 모든 scirpt가 추가되기도 했다.)
 - 이외에도 html 파일을 축약하거나 난독화같은 옵션들을 사용할수도 있다.
 
```javascript
  const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title : 'Index Title',
      hash : true,
      filename : './ht/todex.html',
      // chunks : ['entryname'], // entry에서 해당 리스트를 제외한 나머지.
      excludeChunks: ['entry 설정에서 매칭할 output 이름'] // entry에서 추가시킬 이름
      template: './ht/todex.html'
    }),
    // 여러파일 추가시 아래 처럼 늘려가는 형태가 된다.
    // new HtmlWebpackPlugin({
    //     title : 'Index Title',
    //     hash : true,
    //     filename : './ht/index.html',
    //     // excludeChunks : ['goodBye'], // entry에서 해당 리스트를 제외한 나머지
    //     template: './ht/index.html'
    // })
  ],
};
```


```html
  <button>click</button>
  <script src='./onBtnClick.js'> </script>
```