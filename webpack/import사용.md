# webpack
- - -
또 하나의 헷갈렸던 개념
webpack으로 파일을 합친다고해서 import로 스코프를 동일하게 만들 수 없다. 
조금더 풀어보면
1.js 에서 변수 또는 함수들을 정의해놓고
2.js 에서 1.js를 전체 import 한다고해서 1.js의 함수들을 사용할 수 없다.
- - -

1. 잘못된 사용 
 - webpack으로 두 파일을 합치고 index.html에 사용해보면 결과는 undefined다.

```javascript
  // 1.js
  import $ from 'jquery'
  const a = 'hello';

  // 2.js 
  console.log($)  ->  결과는 undifined
  console.log(a)  ->  마찬가지 결과는 undifined

  // index.html
  <script src='1.js와 2.js가 webpack으로 합쳐진 결과물.js'></script>
```

2. 올바른 사용 
 - 정확히 올바른 사용인지는 정확히 모르겠는데 파악한 바로는 이렇다.

 ```javascript
  // 1.js
  import $ from 'jquery'
  const a = 'hello';
  export $
  export a

  // 2.js 
  import { $ , a } from '1.js';
  console.log($)  ->  jqeury funcion들
  console.log(a)  ->  'hello';

  // index.html
  <script src='2.js를 webpack으로 빌드한 결과물.js'></script>
```