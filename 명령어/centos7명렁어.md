## 자주쓰는 centOs 7 shell 명렁어

1. 방화벽
* firewall-cmd --reload // 방화벽 재시작
* firewall-cmd --zone=public --add-port=8888/tcp --permanent // 방화벽 open 및 추가
* firewall-cmd --list-all // 모든 포트 확인

2. 포트
* netstat -tulpn grep LISTEN // 오픈되어 있는 포트 확인
* iptables -A INPUT -p tcp --dport 22 -j ACCEPT // 포트 오픈 허용
* vi /etc/hosts.deny // 차단할 외부아이피
* vi /etc/hosts.allow // 허용할 외부아이피
* deny에 sshd: ALL (전부 다 막기) 을 추가하고 allow에 허용할 아이피를 sshd: 192.168.0.0 추가하면 allow에 적은 것만 허용 할 수 있음

3. 파일관리
* 복사 붙여 넣기
  - cp [옵션] [원본] [복사본]
  - 옵션
    1 -a 파일의 소유권과 정보를 유지하고 복사대상이 디렉토리일 경우 하위 파일들을 모두 복사
    2 -b 복사할 대상이 있는경우 해당파일을 백업하고 복사
    3 -i 덮어쓸때 물어본다.
    4 -f 덮어쓸때 물어보지 않는다.


3. 기타
* 위에 있는 방법을 다 사용해도 ssh permission 문제가 발생할시
* Setsebool -P httpd_can_network_connect 1 명렁어 입력
* vi /etc/ssh/sshd_confing 에서 permitRootLoigin을 yes로 변경 //root로 ssh 설정 허용 