# CHAP 08

## 함수 정의 및 호출
* 프로시저(Procedure): 한 그룹의 계산과정을 추상화하는 메커니즘.
    * *반환 값이 없음*
    * 매개변수나 비지역 변수를 변경함 
* 함수(Function): *반환 값이 있으므로* 식에 나타날 수 있음
    * 매개변수나 비지역 변수 값 변경은 선택사항 
* 타입 없는 함수 정의
    * Lisp/Scheme, JavaScript, Python은 변수의 타입을 선언하지 않고 바로 사용 가능 -> 함수를 정의할 때도 타입을 선언하지 않음 
* 사례 
    * C,C++,Java,Python,S: 오직 함수만 있고 프로시저는 반환 타입이 void
    * Ada: procedure와 function을 구분함
    * Modula-2: 모두 프로시저. 함수는 반환 값이 있는 프로시저. 

## 매개변수 전달 
* `형식 매개변수(formal parameter)`: 선언에서 사용된 매개변수
* `실 매개변수(actual parameter)`: 호출에서 사용된 매개변수로 인자(argument)라고도 함
* 매개변수 전달: 호출 시에 `실 매개변수`와 `형식 매개변수`를 매칭하는 것 
    * 전달 방법: 값 전달, 참조 전달, 값-결과 전달, 이름 전달 
### 1. 값 전달 pass by value
1. 식(expression)인 실 매개변수 값들을 계산함
2. 계산된 값들을 대응되는 형식 매개변수에 전달(복사 or 초기화)함
3. 본체를 실행함 
* 문제: 값전달만 사용하면 swap이 불가능함 
### 2. 참조 전달 pass by reference
* 참조 전달 방법
1. 실 매개변수의 `위치(주소)`가 계산되어 형식 매개변수에 전달 -> 실 매개변수는 할당된 위치가 있는 변수여야 함 
2. 형식 매개변수 사용: `자동 주소 참조(dereferncing)`가 이루어져 실 매개변수를 접근
    * 형식 매개변수를 변경하면 자동적으로 실 매개변수가 변경됨
* 실 매개변수와 형식 매개변수의 `이명(aliasing)`
* c언어에서 참조 전달 효과 내기 
    * c는 원칙적으로 값 전달만 제공하나 포인터 타입이 있음
    * `참조 전달 효과`: 포인터값(위치/주소)를 명시적으로 전달하여 낼 수 있음
    * `주소 참조(dereferencing)`: 함수 내에서 주소참조는 프로그래머가 명시적으로 해야함
    * 사실상 *프로그래머가 참조 전달을 구현* 
* 참조 전달의 문제점: 이름이 다르면 다른 변수라 생각하기 쉬움. -> aliasing 발생 (하나에게 여러개가 대응될 수 있는 문제)
### 3. 값-결과 전달 pass by value-result (copy-in, copy-out)
* 호출 시: 실 매개변수 값은 형식 매개변수에 전달(복사)
* 반환 시: 형식 매개변수 값을 실 매개변수에 역으로 전달(복사)
* 참조 전달과 다른점: 참조전달과 비슷한 효과낼 수 있지만, *aliasing이 발생하지 않음* (참조전달이 아니기 떄문)
### 4. 이름 전달 pass by name 
* *게으른 계산! 사용될 때까지 버틴다* 
* 이해하는 방법 1: 실 매개변수는 대응 형식 매개변수가 사용될 때까지 계산되지 않고 형식 매개변수가 사용될 때 비로소 계산됨 -> 늦게 계산된다 
* 이해하는 방법 2: 형식 매개변수를 실 매개변수 이름으로 대치하고 실행한다고 생각 할 수 있음 
* 함수형 언어의 `지연 계산(delayed evaluation) (=lazy evaluation)`에서 사용됨 
### 사례 
* pdf 참고.. 

## 함수와 바인딩 
* 선언된 이름(식별자)의 유효범위(scope): 선언된 변수 혹은 함수 이름이 유효한(사용 가능한) 프로그램의 범위 혹은 영역
* 정적 유효범위(static scope) 규칙: 선언된 이름은 `선언된 블록 내`에서만 유효. 대부분 언어에서의 표준 규칙
* 동적 유효범위(dynamic scope) 규칙: 선언된 이름은 `선언된 블록의 실행이 끝날 때`까지 유효함. 실행 경로에 따라 유효범위가 달라질 수 있음 
* 수식 혹은 문장의 의미 파악: **유효한 변수/함수(이름에 대한)에 대한 속성 or 바인딩** 정보가 필요함 
    * 변수의 속성
        * 변수의 선언된 타입
        * 변수 이름의 유효범위
        * 변수의 값 or 위치 
    * 함수의 속성 
        * 함수의 선언된 타입
        * 함수 이름의 유효범위
        * 함수의 코드 위치 
* 바인딩: 이름을 어떤 속성과 연관 짓는 것 
    * 보통 변수,상수,함수 등의 이름(식별자)을 속성과 연관 짓는 것 
    * 정적 바인딩: 컴파일 시에 한번 이루어지고 실행 동안 변하지 않고 유지됨. 정적 바인딩되는 속성은 정적 속성
    * 동적 바인딩 : 실행 중에 이루어지는 바인딩으로 실행 중에 속성이 바뀔 수도 있음. 동적 바인딩되는 속성은 동적 속성 
* 바인딩 정보 관리 
    * 유효한 이름(식별자)
        * 프로그램 각 지점에서 유효한 변수,함수 이름들은 다름
        * 프로그램 각 지점에서 유효한 이름들에 대한 바인딩 정보를 유지 관리해야 함 
    * 컴파일러/인터프리터 
        * 프로그램을 한 줄씩 읽으면서 번역/해석함
        * 프로그램의 각 지점에서 유효한 이름들에 대한 바인딩 정보를 이용하여 번역/해석함 
* `심볼 테이블(Symbol Table)`: **유효한 바인딩 정보를 유지/관리하기 위한 자료 구조.** 환경(environment)라고도 함 
    * 컴파일러: 심볼 테이블에 컴파일하기 위해 필요한 변수 또는 함수의 바인딩 정보 유지 관리 
        * 타입 검사기: 심볼 테이블에 변수 or 함수의 타입 정보 유지 관리 (stack 형태)
    * 인터프리터: 심볼 테이블에 해석해서 실행하기 위해 필요한 변수의 값, 함수의 코드 위치 등 유지 관리 
## 함수의 타입 검사 
* pdf 참고 
* 타입 검사 구현 : 함수 정의
    ```java
    public static Type Check(Function f,TypeEnv te){
        te.push(f.id,new Prototype(f.type,f.params)); //recursion일 경우 함수 타입 추가 
        for (Decl d:f.params)
            te.push(d.id,d.type); 
        Type t=Check(f.stmt,te); //함수 본체 타입 검사 
        for (Decl d:f.params)
            te.pop()
        te.pop() //함수 타입 제거 
        if (t==f.type){ //리턴 타입 비교
            te.push(f.id,new Prototype(f.type,f.params)); //타입 검사된 함수 타입 추가 
            return f.type
        }else{
            error(f,"incorrect return type");
            return Type.ERROR; 
        }
    }
    ``` 
* 타입 검사 구현: 함수 호출
    ```java
    static Type Check(Call c,TypeEnv te){
        if (!te.contains(c.fid)){
            error(c,"undefined function: "+c.fid);
            return c.type
        }
        Exprs args=c.args;
        ProtoType p=(ProtoType) te.get(c.fid); 
        c.type=p.result; 
        if (args.size() ==p.params.size()){ //인수와 매개변수 비교 
            for (int i=0;i<args.size();i++){
                Expr e=(Expr) args.get(i);
                Type t1=Check(e,te);
                Type t2=((Decl) p.params.get(i)).type; 
                if (t1!=t2)
                    error(c,"argument type does not match parameter"); 
            }
        }
        //... 
        return c.type; 
    }
    ```