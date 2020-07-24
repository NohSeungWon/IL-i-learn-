# 현재 모니터 파악

- - -
chrome extension으로 현재 모니터의 개수와 정보를 파악할 수 있다.
api중 system을 사용한다.
- - -

```javascript
chrome.system.display.getInfo((monitors) => {
  console.log(monitors);
  // monitor는 array에 object 형태로 담겨있다. 
  // 아래는 object 예시
  availableDisplayZoomFactors: []
  bounds: {height: 1920, left: 0, top: 0, width: 1080}
  displayZoomFactor: 0
  dpiX: 96
  dpiY: 96
  hasAccelerometerSupport: false
  hasTouchSupport: false
  id: "2528732444"
  isEnabled: true
  isInternal: false
  isPrimary: true
  isUnified: false
  mirroringDestinationIds: []
  mirroringSourceId: ""
  modes: []
  name: "Dell P2219H(DisplayPort)"
  overscan: {bottom: 0, left: 0, right: 0, top: 0}
  rotation: 270
  workArea: {height: 1880, left: 0, top: 0, width: 1080}
});
```