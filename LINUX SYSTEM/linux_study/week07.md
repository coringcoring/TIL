# 인터넷과 서버 

## 1. 네트워크 구성
* LAN(Local Area Network): 근거리 통신망. 집,회사,학교 등의 건물과 같이 가까운 지역을 한데 묶는 네트워크
    * 이더넷: 제룩스 PARC에서 개발된 LAN 구현방법으로 현재 가장 일반적으로 사용되고 있음 
* 라우터(router): 두개 또는 그 이상의 네트워크를 연결하는 장치. 스위치, 공유기라고도 함. 데이터 패킷의 목적지를 추출하여 그 경로에 따라 데이터 패킷을 다음 장치로 보내줌. 
* 게이트웨이(gateway): 고용량 라우터. LAN을 인터넷에 연결하는 컴퓨터나 장치 
    * 무선 액세스 포인트 (Wireless Access Point: WAP): 네트워크에서 와이파이, 블루투스 등을 이용하여 컴퓨터/프린터 등의 무선 장치들을 유선망에 연결할 수 있게 하는 장치 

## 2. 인터넷 
* 인터넷: 전세계의 컴퓨터들이 서로 연결되어 TCP/IP 프로토콜에 따라 서로 정보를 주고받는 공개 컴퓨터 통신망
* 프로토콜: 통신을 하기 위한 규약. 서로 다른 기종의 컴퓨터 사이에 어떤 자료를 어떻게 주고받고 언제 주고 언제 받을지를 정해놓은 규약. 
* TCP/IP 프로토콜
    * IP(Internet Protocol)
        * 호스트의 주소지정과 패킷 분할 및 조립에 대한 규약
        * 인터넷 상의 각 컴퓨터는 고유한 주소인 IP주소를 가짐
        * IP주소는 인터넷 상의 장치들이 서로를 인식하고 통신을 하기 위해 존재함
    * TCP(Trnasfer Control Protocol)
        * IP 위에서 동작하는 프로토콜
        * 데이터의 전달을 보증하고 보낸 순서대로 받게 해줌 
* 호스트명과 IP주소 
    * 호스트명(hostname,domain name): 인터넷에 연결된 컴퓨터에게 부여되는 고유한 이름. 보통 사람이 읽고 이해할 수 있는 이름임. 
    * ip 주소: 실제 사용하는 주소 (8비트씩으로 표현됨)
* 호스트명: `$ hostname` 
    * 사용중인 시스템의 호스트명을 출력 
* IP주소: `$ ip addr` 
    * 사용중인 시스템의 ip주소 출력 

* DNS(Domain Name System)
    * 호스트명을 IP주소로 번역하는 서비스. like 전화번호부 
* nslookup(name server lookup) 명령어 : 도메인 이름 서버(domain name server)에 호스트명에 대해 질의 
    * `$ nslookup 호스트명` : 지정된 호스트의 IP 주소를 알려줌 
* 사용자 정보
    * `$ finger 사용자명`
        * 지정된 사용자에 대한 보다 자세한 정보를 알려줌. 

## 3. 서버 설치 
* 웹서버
    * 리눅스 시스템이 많이 사용되고 있는 분야 중의 하나 
    * 리눅스에 웹 서버가 설치되어 있어야 사용 가능 
* 아파치 웹 서버 
    * 현재 가장 널리 사용되고 있음
    * 우분투: apache2 패키지 설치
    * CentOS: httpd 패키지 설치 
* PHP
    * 웹 프로그래밍 언어
    * 웹 서버와 더불어 사용자의 요청에 따라 동적으로 웹 페이지를 생성하는데 사용
    * 광범위한 데이터를 서비스하기 위해 MariaDB 데이터베이스와 연동하여 사용 
    
* Apache 웹서버, PHP, MariaDB를 총칭하여 `APM` 부름. 

* 아파치 웹 서버 설치
    * sudo 권한으로 해야함
    * `# apt install apache2` 
    * `# apt install php`
    * `# apt install mariadb-server`
    * 구동(start), 서비스활성화(enable), 실행상태(status) 확인
        * `# systemctl start apache2`
        * `# systemctl enable apache2`
        * `# systemctl status apache2` 
    * 방화벽에 http 등록하고(add-service), 바로 적용(reload)
        * `# firewall-cmd --permanent --add-service=http` 
        * `# firewall-cmd -reload`
        * `# firewall-cmd --list-all` 
    * httpd는 기본적으로 var/www/html 디렉터리에서 index.html을 읽어서 웹 브라우저에 디스플레이함 
        * 테스트 페이지 확인: http://ip주소/index.html
        * php 동작 확인: http://ip주소/phpinfo.php 

* ftp 서버 설치 
    * 대표적인 ftp 서버: vsFTPD(very secure File Transfer Protocol Daemon)
    * ftp 서버 설치: `# apt install vsftpd` 
    * vsftpd 시작, 활성화: `# systemctl start vsftpd.service` `# systemctl enable vsftpd.service`
    * 방화벽에 신뢰할 수 있는 서비스로 등록: `# firewall-cmd --add-service=ftp` 

* 원격 접속 서버 ssh
    * 원격 접속
        * 로컬 호스트에서 원격으로 다른 호스트에 접속하여 사용하는 것 
        * telnet은 보안 취약점으로 인해 리눅스에서 제공하지 않음
        * 보안을 강화한 원격 접속 서비스인 ssh(secure shell) 사용
    * ssh 데몬(sshd) 설치 후 시작
        * `# apt install ssh`
        * `# systemctl start ssh`
        * `# systemctl enable ssh`
        * `# systemctl status ssh`

## 4. 파일 전송
* FTP(File Transfer Protocol)(파일 전송 프로토콜)
    * ftp 서버와 클라이언트 사이의 파일 전송을 위한 서비스 
    * 주로 파일을 업로드하거나 다운로드함 
    * `$ ftp 호스트명` 또는 `$ sftp 호스트명` 을 통해 호스트명으로 지정된 ftp 서버에 접속하여 파일을 업로드하거나 다운로드함. 
    * 다운로드 : 대표문자 사용 가능 
        * sftp> get 파일명 
        * sftp> mget 파일명 : 파일 여러개(multiple) 다운받을때 
    * 업로드 : 대표문자 사용 가능 
        * sftp> put 파일명
        * sftp> mput 파일명 : 파일 여러개(multiple) 업로드할때 

## 5. 원격 접속 
* telnet : 내 컴퓨터에서 원격 호스트에 연결하여 사용 가능 
    * `$ telnet 호스트명(혹은 ip주소)` : 지정된 원격 호스트에 원격으로 접속 
* ssh(secure shell) : 안전한 원격 접속. 보안을 위해 강화된 보안 인증과 암호화 기법 사용. 원격 로그인 및 원격 명령 실행 가능 
    * `$ ssh 사용자명@호스트명`, `$ ssh -l 사용자명 호스트명` : 지정된 호스트에 사용자명으로 원격으로 접속 
    * `$ ssh 호스트명 명령` : 원격으로 명령 실행 
* 호스트 확인: `$ ping 호스트명` 
    * 지정된 원격 호스트에 도달 가능한지 테스트하여 상태를 확인 (ip 네트워크를 사용하여 원격 호스트가 도달 가능한지 테스트)

## 6. 원격 데스크톱 연결 
* 원격 데스크톱 프로토콜(RDP: Remote Desktop Protocol): 원격 데스크톱 연결을 위한 프로토콜. 다른 컴퓨터에 GUI 인터페이스를 제공하는 프로토콜 
* 설치: `# apt install xrdp`
* 실행: `# systemctl start xrdp.service` 
* 실행중인지 확인: `# systemctl status xrdp.service`
* 부팅할떄마다 자동으로 실행되도록 하기 : `# systemctl enable xrdp.service`
* 방화벽에서 xrdp 포트를 열어준 후 재시작: `# firewall-cmd --permanent --zone=public --add-port=3389/tcp` `# firewall-cmd --reload` 

## 7. 월드 와이드 웹 
* WWW(World Wide Web): 인터넷에 연결된 컴퓨터들을 통해 사람들이 정보를 공유할 수 있는 전세계적인 정보 공간 
* 하이퍼텍스트(HyperText): 문서 내의 어떤 위치에서 하이퍼링크를 통해 연결된 문서나 미디어에 쉽게 접근 
    * 하이퍼텍스트 작성 언어: HTML(HyperText Markup Language)
* HTTP(Hyper Text Transfer Protocol)
    * 웹 서버와 클라이언트가 서로 통신할 때 사용하는 프로토콜 
    * 웹 문서뿐만 아니라 일반 문서, 사진, 음성, 영상, 등의 다양한 형식의 데이터도 전송
* URL (Uniform Resource Locator): 인터넷에 존재하는 여러가지 자원들에 대한 주소 체계 
* 웹 브라우저
    * WWW에서 정보를 검색하는데 사용하는 소프트웨어 
    * WWW에서 가장 핵심이 되는 소프트웨어
    * 웹페이지 열기, 최근 방문한 URL 및 즐겨찾기 기능 제공 등 
    * 종류: 모자이크, 넷스케이프, 인터넷 익스플로러, 파이어폭스, 사파리, 크롬 
