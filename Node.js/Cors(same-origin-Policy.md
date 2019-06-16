# Node.js express사용시 Cors-orgin에 관한 내용
- - -
* cors안에 origin을 작성하게 되는이유는
* 웹 애플리케이션의 중요한 보안 모델 개념인 동일 출처 정책때문이다.
* <https://www.w3.org/Security/wiki/Same_Origin_Policy/>
* 개발자에게 와닿는 말로 바꿔보면 그 페이지와 같은 서버에 있는 주소로만 ajax 요청을 날릴 수 가 있는 것이다.
* 더 상세하게는 프로토콜, 호스트, 포트가 같다는 것 
- - -

## 1. CORS (Cross-Origin-Resource-Sharing)
* REST API 등 외부호출이 많아짐에 따라 서버에서 외부요청을 허용해주는 정책으로 cors가 대안으로 나왔다.
* 서버에서 외부 요청을 허용하는 것이다.

## 2. PULL CODE
```javascript
app.use(cors({
    origin: 'http://localhost:3000'
  }));
```