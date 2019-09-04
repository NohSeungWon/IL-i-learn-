# Node.js express에서 typeScript setUp하는 방법

## 1. 기본 셋업 (초기 init)

```javascript

npm init -y

npm i --save -dev express
npm i --save -dev typescript

// 모듈을 설치할 때 할상 typeScript를 지원하거나 @types에서 모듈이 있는지 확인하는 것이 좋다.
// 없다면 .d.ts 파일을 만들어야 한다는데 아직 잘 모르겠다.
// @types에 의미를 찾아볼 것
npm -i --save -dev @types/node @types/express

```

## 2. 빌드 설정 & tsconfing.json 설정
* tsconfing.json 파일을 자동생성한다.
* 모든 설정이 다 들어가 있고, 알맞은 것을 선택하면된다.

```javascript
// 자동생성
npx tsc -init

// tsconfing.json 설정
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6",
    "moduleResolution": "node",
    "outDir": "dist"
  },
  "include": [
    "src/**/*"
  ]
}

```

## 3. ts-node 설정
* ts-node는 타입스크립트를 바로 컴파일하고 실행할 수 있게 해준다.
* 그리고 npm start 명령어를 사용하기 위해 pakage.json에 start를 추가해준다.

```javascript
npm install --save -dev typescript ts-node 

// pakage.json
"scripts": {
    "start": "ts-node src/app.ts", // 컴파일할 파일 설정
  },
```

## test
* src폴더에 app.ts파일을 생성하고 
* type error 메세지와 build test를 실행한다.
* require 모듈을 타입이 any라서 typeScript의 기능을 제대로 사용할 수 없다고 한다.
* 그래서 import를 사용하자

```javascript
import * as express from 'express' // 1
const app = express();

app.get('/', (req: express.Request, res: express.Response) => { // 2  
  res.send('Hello TypeScript!');
});

app.listen(3000, () => {
  console.log('port 3000!!');
});

```