# 쉘과 명령어 사용 

## 1. 쉘 소개 
* Shell의 역할 
    * 사용자와 운영체제 사이에 창구 역할을 하는 소프트웨어 
    * 명령어 처리기 command processor
    * 사용자로부터 명령어를 입력받아 이를 처리 
* 유닉스/리눅스에서 사용 가능한 쉘의 종류: 리눅스 처음 생성시 제일 먼저 만들어진 것 `본 쉘` -> 확장된 것: `Bash 쉘`
    * 본 쉘 /bin/sh
        * 벨 연구소의 스티븐 본에 의해 개발됨
        * 유닉스에서 기본 쉘로 사용됨 
    * 콘 쉘 /bin/ksh
        * 1980년대 벨 연구소에서 본 쉘 확장해서 만듦.
    * C 쉘 /bin/csh
        * 버클리 대학의 빌 조이
        * 쉘의 핵심 기능 위에 C언어의 특징을 많이 포함함
        * BSD 계열의 유닉스에서 많이 사용됨
        * 최근에 이를 개선한 tcsh가 개발됨에 따라 사용됨 
    * Bash /bin/bash
        * GNU에서 본 쉘을 확장하여 개발한 쉘
        * 리눅스 및 맥 OS X에서 기본 쉘로 사용되면서 널리 보급됨
        * Bash 명령어의 구문은 본 쉘 명령어 구문을 확장함 
    * tcsh /bin/tcsh
* 로그인 쉘 : 로그인하면 자동으로 실행되는 쉘
    * 보통 시스템 관리자가 계정을 만들때 로그인 쉘 지정 (/etc/`passwd` 안에 계정 정보 들어가있음.)
        * ex> jinyoung:x:109:101:Ubuntu:/home/jinyoung:/bin/bash 
    * 쉘 변경: `$csh`(csh쉘로 변경.)
    * 로그인 쉘 변경: `$ chsh` (암호 입력 후, 변경을 원하는 쉘을 입력해주고 `$ logout` 후 다시 로그인 하면 됨. /bin/csh로 변경할 경우 프롬프트가 $에서 %로 변경됨.)

## 2. 쉘의 기능
* 명령어 처리 : 사용자가 입력한 명령을 해석하고 적절한 프로그램을 실행
* 시작 파일: 로그인할 때 실행되어 사용자별로 맞춤형 사용 환경 설정
* 스크립트: 쉘 자체 내의 프로그래밍 기능 
* 실행 절차
    1. 시작 파일을 읽고 실행한다
    2. 프롬프트를 출력하고 사용자 명령을 기다린다
    3. 사용자 명령을 실행한다
    4. Ctrl+D -> 종료 
* 쉘의 환경 변수 
    * 설정법: `$환경변수명=문자열` 
    * 환경 변수 보기: `$env` 
    * 사용자 정의 환경 변수 
* 쉘의 시작 파일 (start-up file)
    * 시작 파일
        * 쉘마다 시작될 때 자동으로 실행되는 고유의 시작 파일 
        * 주로 사용자 환경을 설정하는 역할을 함
        * 환경설정을 위해서 환경변수에 적절한 값을 설정함.
    * 시스템 시작 파일
        * 시스템의 모든 사용자에게 적용되는 공통적인 설정
        * 환경변수 설정, 명령어 경로 설정, 환영 메시지 출력, ... 
    * 사용자 시작 파일
        * 사용자 홈 디렉터리에 있으며 각 사용자에게 적용되는 설정
        * 환경변수 설정, 프롬프트 설정, 명령어 경로 설정, 명령어 이명 설정, .. 
    * ppt p15 참고하여 공부할 것 
    * 시작 파일 바로 적용: `$ ..bash_profile`

## 3. 전면처리와 후면처리
* 전면처리 : 입력된 명령어를 전면에서 실행하고 쉘은 명령어 실행이 끝날 때까지 기다린다. $ 명령어
* 후면 처리: 명령어를 후면에서 실행하고 전면에서는 다른 작업을 실행하여 동시에 여러 작업을 수행할 수 있다. $ 명령어 & 
    * ex> $ (sleep 100; echo done) & 
    * `$ jobs [%작업번호]`:후면에서 실행되고 있는 작업들을 리스트함. 작업 번호를 명시하면 해당 작업만 리스트한다. 
    * `$ fg %작업번호`: 작업번호에 해당하는 후면 작업을 전면 작업으로 전환시킴. 

## 4. 입출력 재지정
* 출력 재지정(output redirection): `$명령어 > 파일` (명령어의 표준출력을 모니터 대신에 파일에 저장)
    * 간단한 파일 만들기: `$ cat > 파일` (표준입력 내용을 모두 파일에 저장. 파일이 없으면 새로 만듬.)
    * 두 개의 파일을 붙여서 새로운 파일 만들기: `$ cat 파일1 파일2 >파일3` (파일1과 파일2의 내용을 붙여서 새로운 파일3을 만들어줌)
    * 출력 추가: `$명렁어 >>파일` (명령어의 표준출력을 모니터 대신에 파일에 `추가`함)

* 입력 재지정(input redirection): `$ 명령어 <파일` (명령어의 표준입력을 키보드 대신에 파일에서 받음.)
    * 문서 내 입력(here document): `$ 명령어 <<단어`(명령어의 표준입력을 키보드 대신에 단어와 단어 사이의 입력 내용으로 받는다.)

* 오류 재지정: `$ 명렁어2 >파일` (명령어의 `표준오류`를 모니터 대신에 파일에 저장)
    * 명령어의 실행 결과
        * 표준출력 standard output: 정상적인 실행의 출력
        * `표준오류 standard error`: 오류 메시지 출력 

* 파이프: `$ 명령어1 | 명령어2` (명령어1의 표준출력이 파이프를 통해 명령어2의 표준입력이 됨.)


## 5. 여러 개 명령어 실행 
* 명령어 열(command sequence): 나열된 명령어들을 순차적으로 실행함
    * `$ 명령어1; ...;명령어n`
* 명령어 그룹(command group): 나열된 명령어들을 하나의 그룹으로 묶어 순차적으로 실행함. 
    * `$(명령어1;...;명령어n)`: 나열된 명령어들을 하나의 그룹으로 묶어 순차적으로 실행함. 
* 조건 명령어 열(conditional command sequence): 첫번째 명령어 실행 결과에 따라 다음 명령어 실행을 결정할 수 있음. 
    * `$ 명령어1 && 명령어2`: 명령어1이 성공적으로 실행되면 명령어2가 실행되고, 그렇지 않으면 명령어2가 실행되지 않음. 
    * `$ 명령어1 || 명령어2`: 명령어1이 실패하면 명령어2가 실행되고, 그렇지 않으면 명령어2가 실행되지 않는다. 


## 6. 파일 이름 대치와 명령어 대치 
* 파일 이름 대치 
    * 대표문자를 이용한 파일 이름 대치: 대표문자를 이용하여 한 번에 여러 파일들을 나타냄. 명령어 실행 전에 대표문자가 나타내는 파일 이름들로 먼저 대치하고 실행.
        * `*`: 빈스트링을 포함하여 임의의 스트링을 나타냄
        * `?` :임의의 한 문자를 나타냄
        * `[..]: 대괄호 사이의 문자 중 하나를 나타내며 부분범위 사용 가능함
* 명령어 대치(command substitution): 명령어를 실행할 때 다른 명령어의 실행 결과를 이용 (명령어 부분은 그 명령어의 실행 결과로 대치된 후에 실행)
* 따옴표 사용: 따옴표를 이용하여 대치 기능을 제한 
    * 작은따옴표(')는 파일이름 대치, 변수 대치, 명령어 대치를 모두 제한
    * 큰따옴표(")는 파일이름 대치만 제한
    * 따옴표가 중첩되면 밖에 따옴표가 효력을 가짐 