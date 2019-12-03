# sequelize-auto
- - -
sequelize를 쓰다보면 한가지 불편한 점이 define이다.
migration을 사용할 경우로 보면 js 코드에서 define을 
정확하게 db와 맞춰주지 않아도 sync로 동기화 처리를 하는데 문제 없다.
migration 자체도 up, down을 번갈아 하는데 여간 불편함을 느끼는게 아니다.

mysql을 사용할 경우에 workbench와 같이 erd를 생성하고 관리하는데 훨씬
편한 툴들이 존재하기 때문에 굳이 sequelize로 다 관리하지 않기 위해
auto를 사용한다.

방식은 역순이다.
js 상에서 define을 하는 것이 아닌 db에 erd를 불러와
js 파일로 자동 define 코드를 생성해준다.
- - -

1. 설치
npm i sequelize-auto

2. 사용법
사용법은 간단하다.
--save 설치시 node_module > sequelize-auto > bin 을 들어가서
``` javascript
 node sequelize-auto -o '..\..\..\models' -d 디비스키마이름 -h 호스트 -u user이름 -p 포트 -x 비번 -e mysql
```

