# Node.js란
- - -
* Server-Side 개발에 사용되는 플랫폼으로 JavaScript언어를 활용
- - -

## 특징
1. JavaScript 언어로 작성 (Front/ Back을 한가지 언어로 활용가능)
2. Non-Blocking 방식 
3. Single Thread

## 기초 Init Code
```javascript
const http = require('http');

const hostname = '127.0.0.1'; // 안해도 기본
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200; // 안해도 기본설정 
  res.setHeader('Content-Type', 'text/plain'); // 안해도 기본 설정
  res.end('Hello node for study');
});

server.listen(port, hostname, () => {
  console.log(`Server running success!`);
});
```
