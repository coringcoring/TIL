# chap 03

## 프로그래밍 언어 S
### 샘플 프로그래밍 언어의 주요 설계 목표
1. 간단한 `교육용 언어`로 쉽게 이해하고 구현할 수 있도록 해줌
2. `대화형 인터프리터 방식`으로도 동작할 수 있도록 설계
3. 프로그래밍 언어의 `주요 개념`을 쉽게 이해할 수 있도록 설계함. 
    수식, 실행 문장, 변수 선언, 함수 정의, 예외 처리, 타입 검사 등
4. `블록 중첩`을 허용하는 `블록 구조 언어`를 설계한다. 
    전역 변수, 지역 변수, 유효범위 등의 개념을 포함 
5. 실행 전에 `타입 검사`를 수행하는 `강한 타입 언어`로 설계함. 
    `안전한 타입 시스템`을 설계하고 이를 바탕으로 `타입 검사기`를 구현
6. 주요 기능을 점차적으로 추가하면서 이 언어의 **어휘분석기, 파서, AST, 타입 검사기, 인터프리터** 등을 순차적으로 구현 

### 언어 S의 문법 
```
<program> -> {<command>}
<command> -> <decl> | <stmt> | <function> 
<decl> -> <type> id [=<expr>];
<stmt> -> id = <expr>;
        | '{' <stmts> '}'
        | if (<expr>) then <stmt> [else <stmt>]
        | while (<expr>) <stmt>
        | read id; 
        | print <expr>;
        | let <decls> in <stmts> end; 
<stmts> -> {<stmt>}
<decls> -> {<decl>}
<type> -> int|bool|string
```

### 프로그래밍 언어 S
* 언어 S의 프로그램: 명령어 `<command>`들로 구성됨
* 명령어
    * 변수 선언 `<decl>`
        * 변수 타입은 정수(int), 부울(bool), 스트링(string)
    * 함수 정의 `<function>`
    * 실행 문장 `<stmt>`
        * 대입문, 조건 if문, 반복 while문 
        * 입력 read문, 출력 print문 
        * 복합문: 괄호로 둘러싸인 일련의 실행 문장들 
        * let문: 지역 변수를 선언과 일련의 실행 문장들 

## 추상 구문 트리

### 파서와 AST
* **`어휘 분석기(lexical analyzer)`**: **입력 스트링**을 읽어서 **토큰 형태**로 분리하여 반환 
* **`파서(Parser)`**: **입력 스트랑**을 **재귀 하강 파싱**함, 해당 입력의 **AST**를 생성하여 반환 
* **`추상 구문 트리 (Abstract Syntax Tree; AST)`**: **입력 스트링**의 **구문 구조**를 **추상적**으로 보여주는 트리
* **`인터프리터(Interpreter)`**: 각 문장의 AST를 **순회**하면서 각 문장의 의미에 따라 **해석**하여 수행 

### 추상 구문 트리 (Abstract Syntax Tree, AST)
* 입력 스트링의 구문 구조를 추상적으로 보여주는 트리
* 실제 유도 트리에서 나타나는 세세한 정보를 모두 나타내지는 X 
* 수식 Expr의 AST
    * Expr = Identifier | Value | Binary | Unary 
    * Binary (이항연산) 수식
        ```java
        class Binary extends Expr {
            // Binary = Operator op; Expression expr1, expr2
            Operator op;
            Expr expr1, expr2; 
            Binary (Operator o, Expr e1, Expr e2){
                op=o;
                expr1=e1;
                expr2=e2; 
            }// binary
        }
        ```
    * Unary (단항연산) 수식 
* 변수선언의 AST
    * `<type> id = <expr>`
    * AST: Decl = Type type; Identifier id; Expr expr
* 대입문 Assignment의 AST
    * `id= <expr>;`
    * AST: Assignment = Identifier id; Expr expr 
        ```java
        class Assignment extends Stmt {
            Identifier id; 
            Expr expr;
            Assignment (Identifier t, Expr e){
                id = t; 
                expr = e; 
            }
        }
        ```
* Read문, Print문의 AST
* 복합문의 AST
    * 구문법: `<stmts> -> {<stmt>}`      
    * AST: Stmts= Stmt*
        ```java
        class Stmts extends Stmt {
            public ArrayList<Stmt> stmts = new ArrayList<Stmt>(); 
        }
        ```
* If문의 AST
    * 구문법: `if (<expr>) then <stmt> [else <stmt>]`
    * AST: If = Expr expr; Stmt stmt1; Stmt stmt2
* While문의 AST
    * 구문법: `while (<expr>) <stmt>`
    * AST: While = Expr expr; Stmt stmt; 
* Let문의 AST
    * 구문법: `let <decls> in <stmts> end `
    * AST: Let = Decls decls; Stmts stmts; 

## 어휘분석기 구현 
* **`어휘 분석 (lexical analysis)`**
    * **소스 프로그램**을 읽어 들여 **토큰**으로 분리
    * `어휘 분석기` or `스캐너` 
* **`토큰`** : 문법에서 **터미널 심볼**에 해당하는 문법적 단위 
    * **`식별자(identifier)`**: 변수 혹은 함수의 이름 , 토큰 이름: **ID**, 식별자 첫번째는 문자이고 이어서 0개 이상의 문자 혹은 숫자들로 이루어진 스트링 
        * **`정규식(regular expression)`** 형태로 표현 
            * ID = letter(letter|digit)*
            * letter = [a-zA-Z]
            * digit=[0-9]
            * 정규식 ex. NUMBER = digit+ 
    * **`상수 리터럴(constant)`**
    * **`예약어 (keyword)`** : 언어에서 미리 그 **의미**와 **용법**이 지정되어 사용되는 단어 
    * **`연산자(operator)`**
    * **`구분자(delimeter)`**: **EOF** 도 구분자임! 
* 토큰 구현 
    ```java
    enum Token{
        BOOL("bool"), .... 
        EOF("<<EOF>>"), ... 
        ID(""), NUMBER(""), STRLITERAL(""),
        private String value; 
        private Token (String v) { value = v; }
        public String value() { return value; }
    }
    ```
* 어휘분석기 **`getToken()`** 메소드 
    * **호출**될 떄마다 다음 `토큰`을 인식하여 `리턴`함 
    1. 읽은 문자가 **알파벳 문자**: **식별자** or **예약어** 
        * 다음 문자가 알파벳 문자나 숫자인 한 계속해서 다음 문자 읽음
        * 읽은 문자열이 **식별자**인지 **예약어**인지 구별하여 해당 토큰을 리턴 
    2. 읽은 문자가 **숫자**: **정수리터럴**
        * 다음 문자가 **숫자**인 한 계속해서 읽어 **정수리터럴**을 인식 -> 이를 나타내는 **NUMBER 토큰**을 리턴 
    3. 나머지는 읽은 문자에 따라 **연산자**, **구분자** 등을 인식하여 리턴
* 어휘분석 
    * **`Lexer`**: 입력을 읽어서 호출될 때마다 하나의 `토큰`을 반환 
    ```java
    public class Lexer{
        ...
        public Token getToken(){
            예약어(ex if) return Token.IF; 
            ...
            정수 return Token.NUMBER.setValue(s); 
            식별자 return Token.ID.setValue(s); 
            ...
        }
    }
    ```

## 파서 구현 
* 파서: 입력 스트링을 명령어 단위로 파싱하면서 AST를 생성하여 반환 
    ```java
    public class Parser{
        Token token;
        Lexer lexer; 

        public Parser(Lexer l){
            lexer=l;
            token=lexer.getToken(); //처음 토큰 읽기 
        }

        public static void main(){
            // <program> -> {<command>}
            parser=new Parser(new Lexer());
            System.out.print(">>"); 
            do{
                Command command=parser.command(); 
                System.out.print("\n>> "); 
            }while(true); 
        }
    }
    ```
* 파서 구현: 명령어 -> 명령어 (변수 선언, 함수 정의, 문장)를 읽고 파싱하면서 해당 AST를 구성하여 반환함 
    ```java
    public Command command(){
        // <command> -> <decl> | <function> | <stmt> 
        if (isType()){
            Decl d=decl(); 
            return d; 
        }
        if (token==Token.FUN){
            Function f= function(); 
            return f; 
        }
        if (token!=Token.EOF){
            Stmt s=stmt();
            return s; 
        }
        return null; 
    }
    ``` 
* 파서 구현: 변수 선언 
    ```java
    private Decl decl(){
        Type t= type();
        String id=match(Token.ID);
        Decl d=null; 
        if (token == Token.ASSIGN){
            match(Token.ASSIGN);
            Expr e = expr();
            d=new Decl(id,t,e); 
        }else d= new Decl(id,t); 
        match(Token.SEMICOLON);
        return d; 
    }
    ``` 
* Statement 파싱
    ```java
    Stmt stmt(){
        // <stmt> -> <assignment> | <ifStmt> | <whileStmt> | '{' <stmts> '}' | <letStmt> | ... 
        Stmt s; 
        switch(Token){
            case ID:
                s=assignment(); return s;
            case LBRACE:
                match(Token.LBRACE); s=stmts(); match(Token.RBRACE); 
                return s;
            case IF:
                s=ifStmt(); return s; 
            case WHILE:
                s=whileStmt(); return s; 
            case LET:
                s=letStmt(); return s; 
            ... 
            default: error("Illegal statement"); return null; 
        }
    }
    ```
* 파서 구현: Assignment문 
    ```java
    Assignment assignment(){
        Identifier id= new Identifier(match(Token.ID));
        match(Token.ASSIGN); 
        Expr e= expr(); 
        match(Token.SEMICOLON);
        return new Assignment(id,e);  
    }
    ```
* match 함수 : 현재 토큰을 매치하고 다음 토큰을 읽는다 
    ```java
    private String match(Token t){
        String value= token.value();
        if (token ==t)
            token=lexer.getToken(); 
        else
            error(t);
        return value; 
    }
    ```
* 파서구현: 복합문 
    ```java
    Stmts stmts(){
        Stmts ss=new Stmts(); 
        while ((token!=Token.RBRACE)&& (token!=Token.END))
            ss.stmts.add(stmt()); 
        return ss; 
    }
    ```
* 파서 구현: If문
    ```java
    If ifStmt(){
        match(Token.IF);
        match(Token.LPAREN);
        Expr e=expr();
        match(Token.RPAREN);
        match(Token.THEN);
        Stmt s1=stmt();
        Stmt s2=new Empty();
        if (token==Token.ELSE){
            match(Token.ELSE);
            s2=stmt(); 
        }
        return new If(e,s1,s2); 
    }
    ```
* 파서 구현: While문
    ```java
    While whileStmt(){
        match(Token.WHILE);
        match(Token.LPAREN);
        Expr e= expr();
        match(Token.RPAREN);
        Stmt s=stmt();
        return new While(e,s); 
    }
    ```
* let문 파싱
    ```java
    Let letStatement(){
        match(Token.LET);
        Decls ds=decls(); 
        match(Token.IN); 
        Stmts ss=stmts(); 
        match(Token.END);
        match(Token.SEMICOLON);
        return new Let(ds,null,ss); // null은 함수정의를 위해 남겨둔 것
    }
    ```
* 파서구현: 수식 -> 수식을 파싱하고 수식을 위한 AST를 구성하여 리턴
    ```java
    Expr aexp(){
        Expr e=term(); 
        while (token==Token.PLUS || token==Token.MINUS){
            Operator op=new Operator(match(token));
            Expr t=term();
            e=new Binary(op,e,t); 
        }
        return e; 
    }
    ```
* 파서구현: term함수 -> 항(term)을 파싱하고 항을 위한 AST를 구성하여 리턴
    ```java
    Expr term(){
        Expr t=factor();
        while (token== Token.MULTIPLY || token==Token.DIVIDE){
            Operator op=new Oprator(match(token));
            Expr f=factor();
            t=new Binary(op,t,f);
        }
        return t; 
    }
    ```
* 파서구현: factor 함수 
    ```java
    Expr factor(){
        // <factor> -> [-] (number | (<aexp>) | id) | strliteral 
        Operator op=null;
        if (token==Token.MINUS); 
            Operator op=new Operator(match(token)); 
        Expr e=null; 
        switch(token){
            case ID:
                Identifier v=new Identifier(match(Token.ID));
                e=v;
                break; 
            case NUMBER: CASE STRLITERAL:
                e=literal(); break;
            case LPAREN:
                match(Token.LPAREN);
                e=expr();
                match(Token.RPAREN);
                break; 
            default: error("Identifier| Literal");
        }
        if (op!=null) return new Unary(op,e);
        else return e; 
    }
    ```
