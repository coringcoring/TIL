# 프로그래밍 환경 


## 1. 프로그램 작성과 컴파일
* gedit으로 사용 가능 
* gcc 컴파일러 
* 다중 모듈 프로그램 
    * 단일 모듈 프로그램 
        * 코드 재사용 어려움
        * 여러 사람이 참여하는 프로그래밍 어려움 
        * ex> 다른 프로그램에서 copy 함수 재사용 어려움 
    * `다중 모듈 프로그램`
        * 여러 개의 .c 파일들로 이루어진 프로그램
        * 일반적으로 복잡하며 대단위 프로그램인 경우에 적합 

## 2. 자동 빌드 도구 
* `make 시스템`: 대규모 프로그램의 경우 헤더, 소스 파일, 목적 파일, 실행 파일의 모든 관계를 기억하고 체계적으로 관리하는 것 필요 -> make 시스템 이용하여 효과적으로 작업 가능 
* `메이크파일` : 실행 파일을 만들기 위해 필요한 파일들 -> `메이크시스템`이 메이크파일을 이용하여 파일의 `상호 의존 관계`를 파악하여 실행파일을 쉽게 다시 만듬 
    * `$ make [-f 메이크파일]` (그냥 make만 하면 현재 디렉터리 내에 있는 파일들을 메이크파일로 빌드..)
    * 구성형식 (ppt참고)
    
## 3. gdb 디버거
* 가장 대표적인 디버거 
    * 주요 기능
        * 정지점(breakpoint) 설정
        * 한 줄씩 실행
        * 변수 접근 및 수정
        * 함수 탐색
        * 추적(tracing)
    
## 4. 이클립스 통합개발환경

## 5. vi 에디터 