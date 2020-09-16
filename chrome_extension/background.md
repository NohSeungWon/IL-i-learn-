# Background 사용방법

- - -
chrome extension은 background에서 동작할 수 있는 공간이 마련되어있다.
개발을 진행하다가 데이터가 자꾸 유실되는 상황이 생겨 삽질을 오래했는데
background의 lilfeCycle이 있고 그에 따라 데이터가 날라가는 현상이 있기 떄문이었다.

데이터를 유지하고 background를 사용하기 위한 방법이다.
- - -

1. manifest.json 파일을 생성
- background를 사용하기 위해서는 manifest에 다음과 같이 설정한다.

```javascript
  {
    "background": {
      "scripts": ["background.js"], // 작동할 파일
      "persistent": true
      // true로 놔야 백그라운드가 unload되지 않고 유지된다.
    },
  }
```

2. lifeCycle
- 정확히 어떤 라이프 사이클을 타는지는 정확치 않지만 
- runtime 메소드와 관련되서 로직이 일어난다. (https://developer.chrome.com/extensions/runtime)
- 새로고침 되는 시점에 일어나는 runtime은 onSuspend 이고 이때 다시 로드되었을 때 사용할 데이터를 저장해야한다. 저장은 storage를 사용한다.

```javascript
chrome.runtime.onSuspend.addListener(() => {
  // console.log('when', when);
  chrome.storage.local.set({ key : value });

})
```