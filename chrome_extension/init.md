# 현재 모니터 파악

- - -
chrome extension을 개발하기 위해서 초기 셋팅하는 방법
- - -

1. manifest.json 파일을 생성

```javascript
  {
    "name": "GOWIX Screen Contorl", // extesion 이름
    "version": "1", 
    "description": "Build an Extension!", // 설명
    "permissions": ["tabs","system.display"], // api를 사용하려면 이곳에 추가해야한다. chrome.tabs api를 사용하려면 tabs를 추가한 것 과 같이 배열 안에 넣어주면 된다. 
    "background": {
      "scripts": ["jquery.js","background.js"], // background에서 작동할 script를 넣는 곳
      "persistent": false
    },
    "browser_action": {
      "default_popup": "screen.html" // extension을 클릭하면 default로 뜰 html 파일이다. 
    },
    "content_security_policy": "script-src 'self' https://ajax.googleapis.com; object-src 'self'",
    "content_scripts": [
      {
        "matches": ["http://www.google.com/*"],
        "js": ["background.js"] // 확실친 않지만 html 파일에서 <script> 로 로드할 js를 사용하려면 여기에 넣어줘야하는 것 같다.
      }
    ],
    "manifest_version": 2
  }
```