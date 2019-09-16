# CentOS 7 에서 nginx 설치

1. 최신판 설치 및 yum으로 설치를 위해 repo를 생성
* vi /etc/yum.repos.d/nginx.repo 열고
* 아래와 같이 생성 
``` javascript 
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
```
2. Install
* yum install -y nginx

