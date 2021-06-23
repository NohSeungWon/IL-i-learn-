# Scroll Event 기초이해

* 기본사용법 - scroll event 

```jsx
window.addEventListener('scroll', function(e) {
      console.log(e);
});
```

스크롤을 감지하려면 위 함수를 사용하는 것이 기본이다. 여기서 궁금한점은 이벤트의 발생 시점이다.
많은 자료에선 단순히 scroll 이벤트를 저렇게 걸어놓으면 무한히 발생한다고 했는데 정확히는 

1. document의 길이가 현재 웹 화면보다 길이가 길어 스크롤이 생겨있고
2. 그 스크롤이 움직일때 발생한다.
3. 최상단과 최하단에 닿았을경우 이벤트는 발생하지 않는다.
- 발생하지 않는 경우
  documents의 길이가 짧아 스크롤이 생기지 않으면 이벤트가 발생하지 않는다.

  ```jsx
  <body>
    <h1>hello</h1>
    <script>
      window.addEventListener('scroll', function(e) {
        console.log(e);
      });
    </script>
  </body>
  ```
- 발생하는 경우
  document의 길이를 현재 화면보다 크게 주면 스크롤이 발생하고 그 스크롤이 움직일때 발생한다.

  ```jsx

  <body>
    <h1 style="height: 1000px">hello</h1>
    <script>
      window.addEventListener('scroll', function(e) {
        console.log(e);
      });
    </script>
  </body>
  ```

* 간단한 구현방법

문구나, 애니메이션을 동적으로 쉽게 추가, 제거하는 방식을 생각했었는데 많은 생각이 필요한 것 같다...
동적으로 구현하는 방법을 해보기 전 간단한 방법으로 이해도를 높여봐야겠다.

- 스크롤이 존재해야만 스크롤 이벤트가 발생되기 때문에 height값을 미리 반영해 놓아야한다.

    ```jsx
    const container = document.body;
    const a = document.getElementById("container-a");
    const b = document.getElementById("container-b");
    const c = document.getElementById("container-c");

    const getWindowHeight = () => {
      return window.innerHeight;
    };

    const setElHeightToWindowSize = () => {
      a.style.height = `${getWindowHeight()}px`;
      b.style.height = `${getWindowHeight()}px`;
      c.style.height = `${getWindowHeight()}px`;
    };

    setElHeightToWindowSize();
    ```

- 원하는 위치에 이벤트를 발생시키기 위해 스크롤 높이를 % 로 계산해 놓아야한다.

    ```jsx
    /**
     *  @현재 스크롤 위치를 %로 구하는 방법
     * window.scrollY의 시작점이 화면의 끝단이기 때문에 window.innerHeight를 더해야해서 window.scrollY + window.innerHeight를 해준다.
     * 그래서 getPercentOfCureentScrollPosition은 0%로 시작하는게 아니라 끝단의 위치서부터 값이 시작된다.
     */

    const getPercentOfCureentScrollPosition = Math.round(
       ((window.scrollY + window.innerHeight) / document.body.scrollHeight) * 100
    );

    // 몇 퍼센트부터 시작되는지 알려면 아래를 사용한다.

    const scrollStartHeightPercent = Math.round(
      (window.innerHeight / document.body.scrollHeight) * 100
    );
    ```
