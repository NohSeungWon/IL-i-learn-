## msyql 명령어

1. centos
* mysql 구동 관련 : systemctl status mysqld

2. 기본
* mysql user 생성
* - a. 사용자 조회 
* - - use mysql;
* - - select user, host from user;
* - b. 사용자 생성
* - - Create user '사용자'@'ip주소' identified by '비번'; (ip주소 lalhost 가능, %으로 변경하면 모든 외부접속 )
* - c. DB권한 관련
* - - Grant all privileges on *.*  to '사용자'@'ip';
* - - Grant all privileges on DB이름.* to '사용자'@'ip'; 
