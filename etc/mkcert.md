# localhost에서 자체 https 인증서를 만들때

- - -
특이한 경우이긴 하지만 https를 localhost에 적용시킬 때가 있다.
그리고 https가 적용이 되긴하지만 주의 요함이라고 뜨는 것도 벗어나고 싶을 때
자물쇠가 초록인걸 보고싶을 때 ... 사용한다.
- - -

window
------

1. 관리자권한 powershell 실행

2. https://chocolatey.org/install 로 들어가서 command 명령어를 복붙한다.

3. choco install mkcert 실행

4. 관리자 권한으로 cmd에서 원하는 폴더로 들어가 mkcert -install 
 - 그럼 아래와 같은 이미지나오고 예를 클릭

    ![csv](../img/mkcert.png)

5. mkcert example.com "*.example.com" example.test localhost 127.0.0.1 ::1
 - 인증서가 설치될 호스트를 지정하는 것 같다.

6. nodejs 코드
```javascript
const server = https.createServer(
    {
      key: fs.readFileSync('./keys/example.com+5-key.pem'),
      cert: fs.readFileSync('./keys/example.com+5.pem')
    },
    app
  );
```

7. 공유기 ip에서 https 적용
 - https 적용을 하려면 dns나 공유기에서 웹 서버를 띄워야 가능한 것 같다.
 - 그냥 사내에서 사용할 거면 위의 경우를 적용해보면 될 것 같은데 
 - 현재 니즈는 https라기보단 인증되지 않은 인증서로인해 주의문구가 뜨는 것을 해결하는 것이었기에 nginx를 설치하고 
 - proxy pass를 https로 설정하는 것으로 변경했다.