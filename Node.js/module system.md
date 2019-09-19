# NodeJs module system 이해
- - -
이것을 왜 알아야 하는지에 대한 질문으로부터 글을 시작하면 좋을 것 같다.
어떤 서비스의 Server-Side를 NodeJs로 만들어야 한다고 가정해보자
코드를 막 쓰다보면 또는 쓰지 않더라도 설계를 진행하다보면
당연히 폴더 구조와 같이 파일들을 분리하게 된다.
만약 분리하지 않으면 엄청나게 긴 코드를 마주하게 되고 실행중에 오류를 마주하게 되면
으어 끔찍한 상황이 발생할것이다.

안타깝게도 JavaScript는 파일을 분리하지 않는 언어다.
아래와같이 명시적으로 파일을 분리하는 코드를 쓰긴하지만 
```javascript
 <body>
  <script src='file1'>
  <script src='file2'>
  <script src='file3'>
 </body>
```
사실은 한 파일로 구분되어 모두가 다 같은 스코프를 가지게 된다. 
쉽게 보면 file1의 let result 라는 변수가 있고 file 2,3 에도 같은 변수이름이 존재한다면 
문제가 발생할 수 있다는 것이다.

그렇기에, javaScript를 파일단위 혹은 nameSpace단위로 분리하려는 움직임이 지속되어왔고
크게 CommonJs와 AMD가 선두주자로 나서고 있는 상황이며
NodeJs는 CommonJs 방식을 채택하고 있기 때문에 한 파일에 전부 작성하지 않으려면 
commonJS의 module 시스템을 이해하야 한다.
- - -

- 사용법
 * 모듈은 독립적인 파일 스코프를 갖는다. 변수이름이 겹쳐도 괜찮아진다.
 * 모듈에 선언한 항목들을 외부에 공유하려면 exports 객체를 사용해야한다.
 * exports와 moudle.exports란 2가지 방법이 있다.
 * 중요한것은 결국 exports는 module.exports와 동일하다.
 * 왜냐하면 exports가 module.exports를 참조하고 있기 때문이다.

  1. export 방법
  ```javascript 
   // module.js 
  1. 값을 할당할 수 없다.
  exports const a = 1; // 이런식으로 할당 불가능 
  exports = () => a+b; // 결과 {} 기대한 21이 아니다. 값을 할당할수없기 때문
  2. exports 객체에 프로퍼티 또는 메소드로 추가해야한다.
  const a = 11;
  const b = 10;
  exports.sum = () => a + b;
  exports.minus = () => a -b; 

  // import.js 
  const mathWay = require('module.js'); // require로 module.js를 mathway라는 이름으로 할당한다.
  console.log(mathWay) // 걀과: { sum: [Function], minus: [Function] }
  mathWay.sum() // 결과 : 21
  mathWay.minus() // 결과 : 1
  ```

  
  2. module.exports 방법
  ```javascript 
   // module.js 
   1. module.exports 객체에 하나의 값만을 할당할 수 있다. 

   const a = 11;
   const b = 10;
   module.exports = () => a+b; // export = () => a+b; 가 안되는 것과 비교된다.
   // 값이 나오진 않더라도 함수가 정의된다.


  // import.js 
  const mathWay = require('module.js');
  console.log(mathWay) // 결과 [Function]


  2. 여러개 값을 내보내고 싶을경우
  // module.export 객체안에 메소드로 정의한다.
  // module.js 
  const a = 11;
  const b = 10;
  module.exports = {
    sum() { return a+b },
    minus() { return a-b }
  }

  // import.js 
  const mathWay = require('module.js');
  console.log(mathWay) // 결과 { sum: [Function: sum], minus: [Function: minus] }

  ```

  - 중요한 점
   * 가장 중요한 점은 export와 module.exports의 차이를 아는 것이다.
   * export가 결국엔 module.exports를 참조하는 형태이기때문에
   * exports 자체에 값을 할당하면 참조가 깨져버려 moudle기능이 사라진다.
   * console.log(exports)가 {} 인것을 기억하자