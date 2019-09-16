# centOS 7 에서 nginx proxy_pass 설정
- - -
 기본 개념은 nginx에서 요청을 받아
 설정한대로 포트포워딩처럼 해당 port나 경로로 응답을 주게끔 관리하는 것 
- - -

* conf 설정
1. vi /etc/nginx/conf.d/default.conf를 열고
2. 아래와 같이 설정
``` javascript
  location / {
        // 추가한 코드
        proxy_pass http://127.0.0.1:3000/;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real_IP $remote_addr;
        proxy_set_header X-NginX-Proxy true;
        // 아래 부분이 default 
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
```
  - proxy_pass에 경로를 설정해주면 
  - location 에 적힌 / url로 타고 들어올 경우 3000port로 포워딩을 해준다는 의미