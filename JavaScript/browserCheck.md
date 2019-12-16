# browserCheck
웹 접근성을 보장하지 못하고 배포를 진행하거나 운영해야할 때
주로 개발한 브라우저 외의 접근을 차단해야 할 경우가 생긴다.
이때 어떤 브라우저에서 접근하는지를 다음 함수로 체크 할 수 있다.
- - -

## 사용법
```javascript
const  browserCheck = () => {
  const agt = navigator.userAgent.toLowerCase();
  if (agt.indexOf("edge") != -1) return 'Edge';
  if (agt.indexOf("chrome") != -1) return 'Chrome';
  if (agt.indexOf("opera") != -1) return 'Opera';
  if (agt.indexOf("staroffice") != -1) return 'Star Office';
  if (agt.indexOf("webtv") != -1) return 'WebTV';
  if (agt.indexOf("beonex") != -1) return 'Beonex';
  if (agt.indexOf("chimera") != -1) return 'Chimera';
  if (agt.indexOf("netpositive") != -1) return 'NetPositive';
  if (agt.indexOf("phoenix") != -1) return 'Phoenix';
  if (agt.indexOf("firefox") != -1) return 'Firefox';
  if (agt.indexOf("safari") != -1) return 'Safari';
  if (agt.indexOf("skipstone") != -1) return 'SkipStone';
  if (agt.indexOf("msie") != -1) return 'Internet Explorer';
  if (agt.indexOf("netscape") != -1) return 'Netscape';
  if (agt.indexOf("mozilla/5.0") != -1) return 'Mozilla';
  return 'undefined browser';
}
```
* edge에 경우 agt가 chrome을 가장 먼저 캐치하기 때문에 , chrome 보다 앞에 
* if문을 작성해야 정상적으로 캐치할 수 있다.

