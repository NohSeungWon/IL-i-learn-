# nginx http size 관련

1. img 나 대용량 파일을 전송하게 되면 nginx 기본 설정으론 오류가 나타난다.
2. 이때 max_body_size 0; 으로 두면 limit 없이 사용이 가능하다.

