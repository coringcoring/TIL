# CHAP 06

## 자료형의 개요 
* 자료형: 그 타입의 변수가 가질 수 있는 값들의 집합 -> 자료형은 값들의 집합
    * 자료형은 `값들의 집합 + 이 값들에 대한 연산들의 집합` 
    * 수학적 의미
        * an algebra(S,Op) 
            * S: set of values
            * Op: a set of operations on S 
* 기본 자료형 
    * 사전정의된 기본자료형: boolean, int, ...
    * 사용자 정의 기본 자료형 : 열거형 [열거된 값들로 이루어진 타입]
        * C/C++ 열거형 
            ```C
            enum Day {Monday,Tuesday,Wednesday,..};
            enum Day today = Monday;
            ```
        * Ada 열거형 
            ```Ada
            type Day is (Monday,Tuesday,..);
            today : Day; 
            ```
* 부분 타입(subtype)
    * `기반 타입(base type)`의 일부분 값들로 정의된 자료형 
    * 기반 타입의 **모든 연산**을 부분 타입에 적용 가능 
    * C,C++,Java에는 `부분범위형`이 없음 
    * Ada, Pascal의 부분범위형 
        * Ada 예 
            ```Ada
            subtype Digit is Integer range 0..9; 
            subtype Weekday is Day range Monday .. Friday; 
            ```

## 복합 타입 (Complex type)
* `타입 구성자`: 기본 자료형으로부터 더 복잡한 자료형(복합타입)을 구성하는 방법 
    * 배열 타입(Array type)
    * 리스트 타입(List type)
    * 레코드 타입(Record type)
    * 공용체 (Union type)
    * 포인터 타입(Pointer type)
### 구조체(레코드) 타입 
* `구조체`: 여러 개의 `(필드) 변수`들을 묶어서 구성하는 자료형 
    * `struct` in C, `record` in Pascal, Ada,.. 
    * C++의 `class`: C의 struct를 확장. 데이터뿐 아니라 **연산(함수)**들을 포함하도록 확장한 자료형 
### 배열 타입
* 배열 타입: `같은 타입`의 `연속된 변수`들로 구성하는 자료형 
    * 배열 요소(element), 인덱스 
* C의 배열: 배열 크기가 정적 결정 
* 배열 A의 원소의 주소 : `A[i]의 주소 = base + i*sizeof(int)`
    * base는 배열의 시작주소 
* 이차원 배열
    * 자료형 변수이름[M][N]; 
    * **행 우선 배치(row major order) 배치**: 첫번째 행부터 배치하고 이어서 두번째 행을 배치하는 방식
        * C/C++ 언어에서 사용됨 
        * `a[i][j]의 주소=base+(i*4+j)*sizeof(int)`
    * **열 우선(column major order) 배치** : 첫번째 열부터 배치하고 이어서 두번째 열을 배치하는 방식. Fortran 언어에서 사용됨 
* java의 배열 
    ```java
    자료형[] 변수이름;
    자료형 변수이름[];
    ```
    * 주의: 배열 변수를 선언했다고 배열이 자동적으로 만들어지는 것이 아니라, *배열변수는 단지 참조 변수임.* (배열을 참조하는 변수를 만든 것임)
    * `동적 배열`: java 배열은 배열 크기가 실행 시간에 결정되는 일종의 **객체** , 배열 크기가 배열의 일부 
        ```java
        int[] x;
        x= new int[10]; 
        ```
### 리스트 
* 리스트: `항목들의 모음`. 다수의 항목을 `집합적`으로 처리하는데 유용
    * `파이썬`의 대표적인 자료구조. []를 이용하여 정의
    * 파이썬에서는 배열 대신 리스트 사용 
* 리스트는 *배열과 달리 원소들이 같은 타입일 필요가 없음* 
* 리스트의 원소로 리스트도 가능 
* `부분 리스트(slicing)`: 하나의 리스트를 `인덱스`를 사용하여 부분 리스트로 분리 
* 리스트 혹은 `동적 배열`: 배열의 크기가 동적으로 변하는 배열 (java의 ArrayList) 
    * java의 ArrayList는 다양한 타입의 객체 저장 가능 
### 포인터 타입 
* *지금까지 타입들은 위치(주소)를 값으로 사용할 수 없었음* -> 포인터 타입은 메모리의 위치(주소)를 값으로 사용하는 자료형 
    * 포인터 변수 관련 구문
        1. T *p :포인터 변수 선언
        2. p = E: 포인터 변수에 대입
        3. *p=E: 포인터 변수가 참조하는 변수에 대입 
    * 포인터 관련 연산 
        1. malloc(n): 크기 n의 메모리 할당 및 시작 주소 반환
        2. &x: 변수 x의 포인터(주소)
        3. *p: 포인터 변수 p에 저장된 포인터 주소참조 
        ```C
        {
            int *p=malloc(4);
            int y=10; 
            *p=y; //포인터 변수 p에 y의 값 저장 
            *p=*p/2; //포인터 변수의 값을 2로 나누어줌 
            printf("%d: %d\n",p,*p);//27041808: 5
            p=&y;//포인터 변수 p에 y의 주소값을 저장 -> 이제 y를 가리키게됨
            printf("%d: %d\n",p,*p); //944757364: 10 
        }
        ```
### 재귀타입(Recursive Type)
* 타입 정의에 자신의 이름을 사용하는 경우 
    * 재귀 타입을 사용하여 연결리스트 만들 수 있음 
        ```c
        struct CharList{
            char data;
            struct CharList* next; 
        }
        ``` 

## 사례 연구 
* java의 참조 자료형: *인터페이스, 클래스, 배열* 

## 타입 변환
### 자동 형변환 (automatic type conversion)
* `자동 형변환`: 자동으로 형을 변환하는 `묵시적 형변환(implicit type conversion)`
* `확장 변환(widening conversion)`: 표현범위가 더 넓은 쪽으로 변환하는 상향 변환(promotion)
    * java에서 `자동 형변환`은 거의 대부분 `확장 변환` 
    * java 확장 변환 순서 
        byte(1)< short(2) < int(4) < long(8) 
        float(4) < double(8)
### 축소 변환(narrowing conversion)
* `축소변환`: 확장 변환의 반대. 표현 범위가 더 작은 자료형으로 변환 
* `대입 변환(assignment conversion)`: 대입문의 경우에도 확장 변환인 경우에만 자동 수행. 축소 변환인 경우에는 자동으로 수행되지 않음 
    * 예외적으로 int 상수에 대해서만 자동으로 축소 변환됨 
        *short s2 = i; 는 오류다. 상수가 아닌 변수이기 때문!* 
### 형변환 연산자 
* `(자료형) 수식` 
* 수식에 의해 계산된 값을 명시된 자료형의 값으로 변환
* `정보 손실 발생 가능`
* `타입 캐스팅 연산자` 또는 `캐스트`라고 함 

## 타입과 언어 분류 
### 정적 타입 언어 (statically typed language)
* 변수의 타입이 컴파일 시간에 고정되는 언어
* 보통 `컴파일 시간`에 타입 검사를 함
* java, c, c++, fortran, pascal, scala
### 동적 타입 언어 
* 변수의 타입이 `저장되는 값`에 따라 `실행 중`에 바뀔 수 있는 언어
* 보통 실행 시간에 타입 검사를 함 
* Perl, Python, Scheme, JavaScript 등 
* 사례: Python은 변수의 타입이 저장되는 값에 따라 실행 중에 바뀔 수 있으며 변수, 매개변수, 필드에 대한 타입 선언이 없음. *각 값은 타입을 나타내는 타입 태그를 가짐* 
### 강한 타입 언어 
* `타입 규칙`: 언어를 설계할 때 프로그램의 구성요소의 타입 사용 규칙도 정함. 타입 규칙의 엄격성에 따라 강한/약한 타입으로 분류 
* `강한 타입 언어(strongly typed language)`: 엄격한 타입 규칙을 적용하여 `(모든) 타입 오류`를 찾아낼 수 있는 언어 (java, c#, python)
* `약한 타입 언어(weakly typed language)`: 느슨한 타입 규칙을 적용한 언어 (c/c++,PHP,Perl, JavaScript)
### 사례
1. 강한 정적 타입 언어 : Java 
2. 약한 정적 타입 언어 : C
3. 강한 동적 타입 언어: Python
4. 약한 동적 타입 언어: JavaScript
