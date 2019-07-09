# clearInterval
JavaScript의 시간지연 함수를 중지시키는 것
정확히는 현재 실행되고 있는 시간지연 함수가 완료되고 난 후 다음 시간지연 함수들을 실행안되게 하는 것
MDN에 따르면 두 함수모두 같은 ID를 공유하기에 어떤 시간지연 함수를 썼느냐와 상관없이
아무거나 사용해도 동일한 결과를 내나 명시적으로 나눠 쓰는 것을 권장하고 있음 
- - -

## 사용법
```javascript
  // 1초마다 숫자가 +1이 되는 함수를 만든다고 가정
  let increseInterVal;
  let count = 0;
  
  let countUp = () => {
    increseInterVal = setInterVal(plusCount, 1000);
  }

  let plusCount = () => {
    count ++;
  }

  let stopUpCount = () => {
    clearInterVal(increseInterVal)
  }
```
* 무조건 같이 공유하는 increseInterVal 같은 ID가 있어야 정지가 된다.

