# SQL 기본 및 활용 (제 2과목)

## 1. SQL 기본

### [1] 관계형 DB 개요 
1. DB: 데이터를 일정한 형태로 저장해 놓은 것, DBMS(데이터베이스 관리 소프트웨어)를 이용하여 효율적인 데이터 관리와 데이터 손상 복구 가능 
    * 종류 
        * 계층형 DB: 트리 형태의 자료구조에 데이터 저장, 1:N 관계 표현 
        * 네트워크형 DB: 오너와 멤버 형태로 데이터 저장, M:N 관계 표현 
        * 관계형 DB: 릴레이션에 데이터 저장, 집합 연산과 관계 연산 가능 
    * 발전: 플로우차트 -> 계층형, 망형 -> 관계형 -> 객체관계형 
2. 관계형 DB (RDB: Relational Database)
    * 정규화를 통해 이상현상 및 중복 데이터 제거
    * 동시성 관리와 병행 제어를 통해 데이터 동시 조작 가능 
    * 데이터의 표현 방법 등 체계화 할 수 있고 데이터 표준화, 품질 확보
    * 보안기능, 데이터 무결성 보장, 데이터 회복/복구 기능 
    * 집합연산 
        * 합집합(Union)
        * 차집합(Difference)
        * 교집합(Intersection)
        * 곱집합(Cartesian Product; 각 릴레이션에 존재하는 모든 데이터를 조합)
    * 관계연산 [해당 링크 참고](https://satisfactoryplace.tistory.com/213)
        * 선택 연산(Selection): 조건에 맞는 행(튜플) 조회
        * 투영 연산(Projection): 조건에 맞는 칼럼(속성) 조회 
        * 결합 연산(Join): 공통 속성을 사용하여 새로운 릴레이션 생성 
        * 나누기 연산(Division): 공통요소를 추출하고 분모 릴레이션의 속성을 삭제한 후 중복된 행 제거 R%S != S%R 
3. SQL(Structured Query Lnaguage): RDB에서 사용하는 언어. 데이터 조회 및 신규 데이터 입력/수정/삭제 기능 제공 
    * 종류 
        * DML(Data Manipulation Language,데이터 조작어) 
            * SELECT: 데이터 조회 명령어
            * INSERT, UPDATE, DELETE: 데이터 변형 명령어 
        * DDL(Data Definition Language, 데이터 정의어): 데이터 구조 관련 명령어 
            * CREATE, ALTER DROP 
        * DCL(Data Control Language, 데이터 제어어): DB 접근 권한 부여 및 회수 명령어
            * GRANT, REVOKE 
        * TCL(Transaction Control Language, 트랜잭션 제어어): DML로 조작한 결과를 논리적인 작업단위 별로 제어
            * COMMIT, ROLLBACK 
4. 테이블(Table): RDB의 기본 단위, 데이터를 저장하는 객체, 칼럼과 행의 2차원 구조  

### [2] DDL(Data Definition Language)
1. 데이터 타입 
    * 숫자 타입 
        * ANSI/ISO 기준: Numeric, Decimal, Dec, Small Int, Integer, Int, Big int, Float, Real, Double Precision
        * SQL Server/Sybase: 작은 정수, 정수, 큰 정수, 실수 등 + Money, Small Money 
        * Oracle: 숫자형 타입에 대해서 Number 한 가지 타입만 지원
        * 벤더에서 ANSI/ISO 표준을 사용할 땐 기능을 중심으로 구현, 표준과 다른 용어 사용 허용 