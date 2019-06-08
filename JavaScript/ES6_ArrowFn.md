# ES6 How to use Arrow Functinos

```javascript
// 기존 함수 선언시
  function(){ };
  // 화살표 선언
  (){ };
 
  // 기존함수 이름이 있는 경우
  var es5 = function () { };
  // 화살표 함수 이름 있는 경우
  var es6 = () => { };
 
 
  // 함수안에 저장된 값이 함수에 파라미터로 넣는 값보다
  // 크면 true를 반환// 작으면 false를 반환하는 함수 작성 테스트
 
  // 기존
  var es5 = function (num) {
    if(num < 10) {return true;};
    return false;
  }
 
  // 화살표
  var es6 = (num) => {
    if(num < 10) { true;};
    return false;
  }
 
  // 결과
  // es5(9) = true;
  // es6(9) = tue;
 
 
  // 메소드 함수로서 작동할 때 주의할 점
  // this가 바인딩 되지 않습니다.
  
  // 기존 
  var es5 = {
    name : "hello",
    func : function () {
      console.log(this.name)
    }
  }
  // 화살표
  var es6 = {
    name : "hello",
    func : () => { return console.log(this.name)}
  }
 
  // 함수 축약형으로 사용
  var short_es6 = {
    name : "hello",
    func(){
      return console.log(this.name);
    }
  }
 
  // 결과
  es5.func()  = "hello";
  es6.func()  = undefined;
  short_es6.func() = "hello"
```