# NodeJs import 사용법 (ES2015)
- - -
https://github.com/NohSeungWon/IL-i-learn-/blob/master/Node.js/module%20system.md
위 글에 연장선상으로 require과 비교를 통해 import로 module system을 사용하는 방법을 알아보자
ES2015 부터 import가 도입되었고, 파일 최상단에 명시적으로 필요한 파일들을 선언해야한다.
- - -

- 사용법
 * 파일 최상단에 명시적으로 필요한 파일들을 선언한다.
 * export와 export default 2가지 방식을 사용한다.
 * 코드 
  ```javascript 
   // module.js 
  const a = 1;
  const b = 2;
  export { a };
  export const c = 3;
  export default b;

  // import.js 
  import d, { a, c as e } from './module';
  console.log(a, d, e); // 1, 2, 3
  ```
  1. import d 
   * export default b 의 방법으로 배출된 변수는 {} 괄호를 쓰지 않아도 import 할 수 있다.
   * 또한 이름을 변경해서 사용할 수 있는데, import d = export default b 란 의미이다.
  2. import { a, c as e } from '.module;
   * default를 사용하지 않으면 {괄호}를 사용하여 불러야 하고, 이름도 같아야 한다.
   * 이름을 바꾸려면 c as e 처럼 사용해야한다. 