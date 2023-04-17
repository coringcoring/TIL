# chap 04 

## 변수 선언
* **`변수 선언`**: 변수는 사용 전에 먼저 선언되어야함 -> **사용 전 선언**(**`declaration before use`**)
* 변수의 **`유효 범위(scope)`**: 선언된 변수가 유효한(사용될 수 있는) 프로그램 내의 범위/영역 , 변수 이름 뿐만 아니라 **함수** 등 다른 이름도 생각해야 
* **`정적 유효범위(static scope) 규칙`**: **선언된 이름은 선언된 블록 내에서만 유효.** 대부분 언어에서 *표준 규칙*으로 사용됨 
* 구문법
    ```
    <stmts> -> ...
            | id = <expr>; 
            | let <decls> in <stmts> end; 
    <decls> -> { <type> id [= <expr>]; }
    <stmts> -> {<stmt>}
    <type> -> int | bool | string 
    ```
    * 초기화하지 않는 변수는 자동으로 기본값(0,"",false)로 초기화됨 
    * 변수 id는 지역 변수로 유효범위는 선언된 블록 내. 
* 블록의 중첩 : let 블록 내의 문장 S에 다시 let 블록이 나타날 수 있음 
    * 예시들 ppt 참고 
* `지역 변수`: let문 내에서 선언된 변수
* `전역 변수`: let문 **밖**에서 선언된 변수는 전역 변수(**global variable**)
* 타입 **없는** 변수 선언 
    * **`동적 타입 언어(dynamically typed language)`**: 변수의 타입을 선언하지 않고 바로 사용. 변수에 어떤 타입의 값이든지 가능 (ex. Lisp/Scheme, JavaScript, Python 등)
    * 주의: 함수 내에서 전역 변수 **사용**은 가능하나 전역 변수 **수정**은 불가능함. (이유: **전역 변수에 대입**하면 자동으로 **지역 변수**가 생성됨)
        * python에서 `UnboundLocalError: local variable`이라는 에러가 발생
        * 따라서 `global` 을 통해 함수 내에 전역 변수를 설정하여 값을 변경 해야함 (예시 ppt 참고)

## 블록 구조 언어 
* 블록: 서로 **연관**된 `선언문`과 `실행문`들을 묶어놓은 프로그래밍 단위
    * 변수나 함수를 선언하는 선언문들과 실행문들로 구성됨 
* 블록 구조 언어: **블록의 중첩**을 허용하는 언어 
    * 특징 및 장점 
        1. 대형 프로그램을 여러 블록으로 나누어 작성하면 복잡한 수행 내용을 단순화하여 프로그램의 해독성을 높여줌
        2. 프로그램 오류가 발생하여도 그 범위가 블록단위로 한정되므로 수정이 쉬워지며 블록의 첨가, 삭제, 수정이 용이
        3. 블록 내에 선언된 변수들은 그 안에서만 유효하며 실행이 종료되면 기존에 선언되었던 변수들은 모두 무효화 
        4. 사용자로 하여금 변수의 사용과 기억장소의 할당에 관한 경계를 명확히 가능 
* C언어의 유효범위 규칙 
    * 핵심 아이디어: **`사용 전 선언(Declaration before use)`**, 선언의 유효범위는 선언된 시점부터 선언된 블록 끝까지 
    * 지역 변수의 유효범위: 선언된 지점부터 **함수 끝**까지 
    * 전역 변수의 유효범위: 선언된 지점부터 **파일 끝**까지 
    * 예제들은 ppt를 참고.. (다 적기에는 시간이 부족)
* Ada 언어의 유효 범위 규칙: 선언의 유효범위는 선언된 블록 내 

## 변수의 상태와 의미 
* 변수: **`메모리 위치(주소)`**를 나타내는 이름 
* x = x + 1; 
    * 오른쪽 변수 x의 의미: 메모리 위치에 **저장된 값(r-value)**
    * 왼쪽 변수 x의 의미: **메모리 위치 (l-value)**
* **`상태 (State)`**: **변수들의 현재 값**. 하나의 `함수`로 생각할 수 있음 
    * s: Identifier -> Value 
    * ex> s= { x->1, y->2, z->3 }
    * 모든 가능한 상태들의 집합 : State= Identifier -> Value
* `수식 E`의 의미 : **상태에서 수식의 값** 
    * V: (State,Expr) -> Value 
    * 예시들 ppt 참고 
* 문장의 의미 
    * **문장 S**가 `전상태 s`를 `후상태 s'` 로 변경시킴 (**`상태변환`**)
    * **`상태 변환 함수(state transform function)`**
        * Eval: (State, Statement) -> State 
    * 의미론: 각 문장 S마다 상태 변환 함수 정의, 프로그램의 실행 과정을 상태 변환 과정으로 설명 

## 변수의 유효범위 관리 
### 블록 구조를 위한 상태관리 
* 블록 시작을 만났을 때 
    * 블록 내에 선언된 변수는 유효해짐
    * 선언된 변수에 대한 상태 정보를 새로 생성 
* 블록 내 문장을 만났을 때
    * 유효한 변수들의 상태 정보를 이용해서 문장을 해석(실행)함
* 블록 끝을 만났을 때
    * 블록 내의 선언된 변수들은 더 이상 유효하지 않음
    * 블록 내의 선언된 변수들의 상태 정보를 제거 
* **상태(state)를 스택(stack) 형태로 유지 관리**
    * First-In Last-out
    * Last-in First-out

## 구현 
### 상태 구현
* 상태: 변수와 그 값을 대응시키는 하나의 함수 
* s: Identifier -> Value
* 변수의 값을 나타내는 <변수 이름, 값> 쌍들의 집합으로 표현 -> `<id,val>`
* 스택 형태로 유지 관리 : 변수의 유효범위를 동적으로 관리하기 위함. <변수이름, 값> 쌍들의 스택 
    ```java
    class State extends Stack<Pair>{
        //id는 식별자. 변수이름 나타냄
        //생성자 
        public State();
        public State(Identifier id, Value val);

        public State push(Identifier id,Value val);
        public Pair pop(); 
        public int lookup(Identifier id); //있는지 없는지 찾아보는 것 

        public State set(Identifier id, Value val); //상태에서 변수 값 설정
        public Value get(Identifier id);  // 상태에서 변수값 조회 
    }
    ```
### 수식의 값 계산 
```java
Value V(Expr e,State state){
    if (e instanceof Value) return (Value) e;
    if (e instanceof Identifier){
        Identifier v=(Identifier) e; 
        return (Value)(state.get(v)); 
    }
    if (e instanceof Binary){
        Binary b=(Binary) e;
        Value v1=V(b.expr1,state);
        Value v2=V(b.expr2,state); 
        return binaryOperation(b.op,v1,v2); 
    }
    if (e instanceof Unary){
        Unary u=(Unary) e;
        Value v= V(u.expr,state);
        return unaryOperation(u.op,v); 
    }
}

Value binaryOperation(Operator op,Value v1,Value v2){
    switch(op.val){
        case "+":
            return new Value(v1.intValue()+v2.intValue()); 
        ...
    }
}
```
### 대입문 실행 
* 상태 변환 함수
    ```java
    State Eval(Assignment a, State state){
        Value v=V(a.expr,state); 
        return state.set(a.id,v); 
    }
    ```
### let문 실행 
* 상태 변환 함수 
    ```java
    State Eval(Let l,State state){
        State s=allocate(l.decls,state);
        s=Eval(l.stmts,s);
        return free(l.decls,s); 
    }
    ```
    * allocate 함수: 선언된 변수들(ds)을 위한 엔트리(pair)들을 상태 state에 추가 
        * 엔트리 추가 함수: State push(Identifier id,Value val)
    * free 함수 : 선언된 변수들(ds)의 엔트리(pair)를 상태 state에서 제거 
### 전역 변수 선언
* 상태 변환 함수
    ```java
    State Eval(Command p,State state){
        if (p instanceof Decl){
            Decls decls=new Decls();
            decls.add((Decl)p);
            return allocate(decls,state);
        }
        if (p instanceof Stmt){
            return Eval((Stmt)p,state); 
        }
    }
    ```