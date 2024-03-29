# CHAP 01

## 프로그래밍 언어? 
* 계산 과정을 기계가 읽을 수 있고 사람이 읽을 수 있또록 기술하기 위한 일종의 표기법 

## 프로그래밍 언어의 역사
### 1950년대: 고급 프로그래밍 언어의 시작 
* FORTRAN
* COBOL 
* LISP 
### 1960년대: 프로그래밍 언어의 다양성
* Algol60/58
* Algol680
* PL/1
* Simula-67
* BASIC
### 1970년대: 단순성 및 새로운 언어의 추구 
* PASCAL
* C
* Prolog
* Scheme
* ML
### 1980년대: 추상 자료형과 객체지향
* Ada
* Modula-2
* Smalltalk 
* C++
### 1990년대: 인터넷 언어와 새로운 시도 
* Python
* Java
* JavaScript 
### 2000년대: 새로운 미래를 향하여 
* C#
* Scala
* Objective-C와 Swift

## 추상화와 명령형 언어의 발전 
* 폰 노이만 모델 컴퓨터 
    * 프로그램 내장 방식 컴퓨터 
    * 메모리에 저장된 명령어 순차적으로 실행 
    * 명령어: 메모리에 저장된 값을 조작 및 연산 
    * CPU의 인출-해석-실행 주기 반복: cpu는 메모리 내에 저장되어 있는 프로그램의 명령어를 한 번에 하나씩 가져와서 해석하고 실행
* 명령형 언어(Imperative language)
    * 폰 노이만 모델 컴퓨터의 특징을 많이 갖고 있음 
* 추상화(abstraction): 추상화는 실제적이고 구체적인 개념들을 요약하여 보다 높은 수준의 개념을 유도하는 과정 (컴퓨터의 데이터, 연산, 명령어 등을 추상화 - 데이터 추상화/제어 추상화)
    * `데이터 추상화`: 변수, 자료형, 구조적 추상화, 배열, 레코드(구조체) 
    * 제어? : 코드들의 실행 순서. (호출 순서) (EX. for문..)
    * `제어 추상화`
        * 기본 추상화: 몇 개의 기계어 명령어들을 하나의 문장으로 요약 (ex.대입문, goto문)
        * 구조적 제어 추상화: 테스트 내의 중첩된 기계어 명령어들을 하나의 문장으로 요약 
        * 프로시저(함수, 메소드)
            * 선언: 일련의 계산 과정을 하나의 이름으로 요약하여 정의
            * 호출: 이름과 실 매개변수를 이용하여 호출 
    * 추상 자료형: 데이터+ 관련 연산 / 데이터와 관련된 연산들을 캡슐화하여 정의한 자료형 

## 프로그래밍 언어의 정의 및 구현 
