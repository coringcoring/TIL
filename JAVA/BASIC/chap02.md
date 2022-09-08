# CHAP02 STUDY

## 자바 프로그램 구성 요소 

```java
/*덧셈 프로그램*/  //주석 
public class Add //클래스 정의 
{
    public static void main(String[] args){ //메소드 정의 
        int x,y,sum; //변수 선언 
        x=100; //대입(할당) 연산
        y=200;

        sum=x+y;
        System.out.println(sum); //출력문  
    }
}
```

### 클래스 class
* class: 자바와 같은 객체 지향 언어의 기본적인 빌딩 블록 
* 자바에서 소스 파일 이름은 항상 `public`이 붙은 클래스의 이름과 동일하여야 함. 
* public class는 오직 하나, main 메소드를 가지고 있는 class도 오직 하나여야. 
    * 하나의 소스 파일에 여러개의 클래스가 들어 있는 경우
        * public 클래스 하나 있을 경우: public 클래스명
        * public 클래스가 없을 경우: 소스 파일 안에 포함된 아무 클래스명 
        * public 클래스가 2개 이상 있을 경우: 컴파일 에러 발생 

### 메소드 method
* 특정한 작업을 수행하는 코드의 묶음 
* like 클래스 안에 정의된 함수 

### 문장 statement 
* 사용자가 컴퓨터에게 작업을 지시하는 단위 
* 프로그램을 이루는 가장 기초적인 단위 

### 주석 comment 
* 소스코드가 하는 일을 설명하는 글. 프로그램의 실행결과에 영향 끼치지 않음. 

```java
/* text */ 
```
여러 줄 주석처리 시 사용


```java
//text
```
한 줄 주석처리 시 사용


---

## 변수와 자료형 
* 변수 variable: 데이터를 담아두는 상자로 생각 

### 식별자 만들기 
* 알파벳 문자, 숫자, 밑줄 문자(_)로 이뤄짐. 한글 이름도 가능(but 영어 권장.)
* 첫번째 문자는 반드시 알파벳 or 밑줄문자(_). `숫자로 시작 불가능`
* %, &, # 와 같은 특수문자 사용 불가능. but, `$` 와 `_` 는 `가능`.
* 대문자와 소문자 구별함.
* 자바 언어 키워드 (if,while,super,...) 불가능 

* 식별자의 관례 
    1. 클래스명: 각 단어의 첫글자는 대문자로. (ex. Add, StaffMember)
    2. 변수명, 메소드명: 첫단어의 첫글자는 대문자. 두번째 단어부터는 단어 첫글자를 대문자로. (ex. width,acctNumer)
    3. 상수: 모든 글자를 대문자로. (ex. MAX_NUMBER)


### 자료형 data type
* 자료형 data type: 변수에 저장되는 데이터의 타입. 
    * 기초형 primitive type: 정수형, 실수형, 논리형, 문자형 
    * 참조형 reference type: 클래스, 인터페이스, 배열 

* 기초형 primitive type

    | 자료형 | 설명 | 크기 | 범위 | 
    |-----| --- | --- | ----- |
    |byte|정수|1byte|-128~127|
    |short|정수|2byte|-12768~32767|
    |int|정수|4byte|-2147483648~2147483647|
    |long|정수|4or8byte|-2^63 ~ (2^63 - 1)|
    |float|부동소수점형|4byte|약 +-3.40282347x10^(+38)|
    |double|부동소수점형|8byte|약+-1.7976931x10^(+308)|
    |char|문자형|2byte|\u0000~\uFFFF|
    |boolean|논리형|1byte(실제로는1bit로도 충분)|true,false|

### 문자형 char
* char은 하나의 문자 저장 가능. 
* 자바에서는 모든 문자를 2byte의 유니코드unicode로 나타냄. 
```java
char ch1 ='가'; //2byte
char ch2 = '\uac00'; //'가'를 나타내는 유니코드 \u는 유니코드를 표현하는 의미.
char ch3 = 'a'; //2byte
```

### 리터럴 literal
* 리터럴literal에는 정수형, 부동소수점형, 문자형 등의 여러 타입이 존재. 
    * 10진수 decimal : 14,16,17
    * 8진수 octal : 012,013,014 
    * 16진수 hexadecimal: 0xe, 0x10, 0x11
    * 2진수 binary: 0b1100 <-JDK7부터 이진수 표현. 

### 논리형 boolean 리터럴
* 논리형 boolean type은 참,거짓을 나타내는데 사용
* 논리형은 true 또는 false만을 가질 수 있음. 
```java
boolean flag = true;
boolean x = 1<2; //false가 저장됨.
```

### 상수 constant
* 상수 constant: 프로그램이 실행하는 동안 값이 변하지 않는, 변경불가능한 수를 의미. 
```java
final double PI=3.141592; 
```
* final은 상수를 선언하는 키워드. double은 자료형. PI는 상수 이름. 3.141592는 상수값 
* 변경 시도시 오류 발생 

### 변수 타입 추론 
* Java 10 부터는 `var` 키워드 사용 가능. 지역 변수의 타입을 자동으로 추론하는 것이 가능. 
```java
var age = 22; //age는 int타입으로 추론.
var name = "Kim"; //name은 String타입으로 추론 

// MAP<String,String> map = new HashMap<String,String>(); 을 아래와 같이
var map= new HashpMap<String,String>(); 
//map을 Map<String,String>타입으로 추론한다. 
```
* 컴파일러가 지역 변수 유형을 추론하기에 충분한 정보가 없을 경우, 컴파일에 실패한다. 예시는 아래와 같다. 
```java
var sum; //컴파일 실패. 
```

#### 예제 
* 1광년 거리 계산
```java
public class Light{
    public static void main(String args[]){
        final double LIGHT_SPEED=3e5;
        double distance;

        distance= LIGHT_SPEED*365*24*60*60;
        System.out.println("빛이 1년동안 가는 거리"+distance+" km");  
    }
}
```
```java
//실행결과
빛이 1년동안 가는 거리: 9.4608E12 km
```

* 원의 면적 계산
```java
public class AreaTest{
    public static void main(String args[]){
        final double PI=3.141592;
        double radius,area;

        radius=5.0;
        area=PI*radius*radius;
        System.out.println("반지름이 5인 원의 면적은 "+area);
    }
}
```
```java
//실행결과
반지름이 5인 원의 면적은 78.5398
```

### 문자열 String
* 문자열 String: 문자들의 모임 
```java
String s1="Hello";
String s2="World!";

System.out.println(s1+s2); //Hello World!가 출력됨. 

int age=20;
System.out.println("내년이면 "+age+"살"); //내년이면 20살 이 출력됨. 
```

* 아래는 어떻게 출력될까? 
```java
System.out.println(1+2+"3"+4+5);
```
과정을 설명하자면, 컴퓨터는 모든 연산자들이 +로 서로 동등하게 계산되어야함을 인식하고, 왼쪽에서 오른쪽으로 차례대로 값을 읽어나간다. 따라서, 

1+2를 하여 3
3+"3"이 되므로 "33"이 되고,
"33"+4+5가 되므로
`"3345"`라는 출력값이 나오게 된다.

### 형변환 type conversion 
* 컴퓨터에서 산술적인 연산을 하기 전에 피연산자의 타입을 통일함. 
* 이때 가장 범위가 넓은 피연산자의 타입으로 변환됨. 
```java
double sum =1.3+12; //1.3+12.0으로 변환됨. 
``` 

* 강제 형변환
```java
int x=3;
double y = (double)x;
```

```java
int x=3;
double y=x; //이 또한 강제형변환에 해당. 

y=20.7;
x=y; //C컴파일러의 경우 값손실 경고 메시지만 띄우고 .7의 값의 손실을 일으킴. 그러나 컴파일은 정상적으로 작동됨. 그러나 java는 컴파일 에러가 뜬다. 

//따라서
x=(int)y; //처럼 선언해야함. 
```

#### 예제 
* 형변환
```java
public class TypeConversion{
    public static void main(String args[]){
        int i;
        double f;

        f=1/5;
        System.out.println(f); //정수연산. 0을 실수값 double에 넣어서 0.0 출력.

        f=(double)1/5; 
        System.out.println(f); //실수와 정수 연산. 자동변환으로 실수 연산으로 바뀜. 1.0/5.0 이므로 0.2로 출력.

        i=(int)1.7+(int)1.8;
        System.out.println(i); //1+1로 계산됨. (소수점 뒤의 값 손실.int로 강제변환.) 2로 출력. 
    }
}
```


---

## 콘솔에서 입력 받기 
* 콘솔에서 읽는 것은 `System.in` 사용. System.in은 키보드에서 바이트를 읽어서 우리에게 전달. 
```java
import java.util.Scanner; //Scanner 클래스를 포함시킴.
Scanner sc = new Scanner(System.in); //Scanner 클래스의 객체를 생성. 
``` 

```java
//사용자로부터 두 수 입력받아서 더하기
import java.util.Scanner;

public class Add2{
    public static void main(String args[]){
        Scanner sc= new Scanner(System.in);
        int x,y,sum;

        System.out.print("첫번째 숫자를 입력하시오: "); //줄바꿈x 
        x=sc.nextInt(); //only int possible. 
        
        System.out.print("두번째 숫자를 입력하시오: ");
        y=sc.nextInt();

        sum=x+y; 
        System.out.println(sum); //합을 출력한다. 
    }
}
```
```java
//실행결과
첫번째 숫자를 입력하시오: 10
두번째 숫자를 입력하시오: 20
30
```

### import 문장 
* 자바에서 모든 클래스는 사용 전 import되어야함. 
* 기본적인 기능들은 java.lang 패키지 안에 저장되어 있고, 이들은 import 필요 없음. 
* 이클립스에서 import 쉽게 할 수 있는 기능 제공. 
    * 오류 표시된 문장 위에 커서 올리고 있으면 QuickFix 기능 이용 가능. 
    * 소스파일 전체에 등장하는 모든 클래스 import: `Shift+Ctrl+O` 

### Scanner 클래스 
* Scanner 클래스는 키보드로부터 바이트 값을 받아서  `분리자`를 이용하여 각 바이트를 `token` 으로 분리함. 
* 특별한 지정 없으면 분리자는 `공백문자('','\n','\t')`.
```java
//if "Kim 20 84.0"을 입력시, 
String name=sc.next(); //한 단어(토큰) "Kim"을 읽음.
int age= sc.nextInt(); //문자열 "20"을 정수 20으르 변환하여 읽음.
double weight=sc.nextDouble(); //문자열 "84.0"을 실수 84.0으로 변환하여 읽음.

//또는 한줄로 쭉 읽게 할 수 있다. (주소와 같은 경우 많이 사용)
String line = sc.nextLine(); //문자열 "Kim 20 84.0"이 반환됨. 
```

#### 예제 
* 사용자로부터 이름, 나이 받는 프로그램
```java
import java.util.*;

public class InputString{
    public static void main(String args[]){
        String name;
        int age;
        Scanner sc=new Scanner(System.in);

        System.out.print("이름을 입력하시오: ");
        name=sc.nextLine();
        System.out.print("나이를 입력하시오: "); 
        age=sc.nextInt(); 

        System.out.println(name+"님 안녕하세요! "+age+"살이시네요"); 
    }
}
``` 

---
# 수식과 연산자 
* 수식= 피연산자+연산자
* 연산자(operator): 특정한 연산을 나타내는 기호
* 피연산자(operand): 연산의 대상 

### 연산자 
* 우선순위는 교재에 있는 표를 참고. (p68)

### 예제
* 나머지 연산자 예제. 초 단위의 시간 받아서 몇분 몇초인지를 계산하여 출력하는 프로그램. 
```java
import java.util.Scanner;

public class CalTime{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);

        System.out.print("초를 입력하시오: ");
        int time=sc.nextInt();
        int sec=(time%60);
        int min=(time/60);

        System.out.println(time+"초는 "+min+"분 "+sec+"초입니다."); 
    }
}
```

* 증감, 복합 대입 연산자 
```java
public class IncOperator{
    public static void main(String args[]){
        int x=1,y=1;

        int a=x++;
        int b=++y;
        System.out.println("a="+a+" b="+b);

        int c=100,d=200;
        c+=10;
        d/=10;
        System.out.println("c="+c+", d="+d); 
    }
}
```


### 관계 연산자 relational operator
* 두 개의 피연산자 비교시 사용
* ==, !=, >, <, >= 등등.. 


### 논리 연산자 
* 여러 개의 조건을 조합하여 참인지 거짓인지를 따질 때 사용. 
* &&, ||, ! 

### 예제 
```java
public class CompOperator{
    public static void main(String[] args){
        System.out.print((3==4)+" "); 
        System.out.print((3!=4)+" ");
        System.out.print((3>4)+" ");
        System.out.print((3<4)+" ");
        System.out.print((3==3 &&4==7)+" ");
        System.out.print((3==3||4==7)+" ");
    }
}
```
```java
//실행결과
false true false true false true
```

### 비트 연산자 
* ~,&,^,| 
* 프로그램과 하드웨어 칩 간의 통신에 사용됨. (ex. 세탁기 안에 있는 8개의 센서.)
```java
public class BitOperator{
    public static void main(String args[]){
        byte status=0b01101110;

        System.out.print("문열림 상태="+(status&0b00000100)!=0); 
    }
}
```
```java
//실행결과
문열림 상태=true
```

### 비트 이동 연산자
* << : 비트 왼쪽 이동.
* >> : 비트 오른쪽 이동.
* >>> : 비트 오른쪽 이동 (unsigned): 왼쪽 비트가 부호 비트로 채워지지 않고 0으로 채워짐. 

```java
public class BitOperator2{
    public static void main(String[] args){
        int x=0b00001101; //13
        int y=0b00001010; //10

        System.out.print("x&y="+(x&y)+"   ");
        System.out.print("x|y="+(x|y)+"   ");
        System.out.print("x^y="+(x^y)+"   ");
        System.out.print("~x="+(~x)+"   ");
        System.out.print("x>>1="+(x>>1)+"   ");//13/2=6 
        System.out.print("x<<1="+(x<<1)+"   ");//13*2=26 
        System.out.print("x>>>1="+(x>>>1)+"   ");
    }
}
```
```java
//실행결과
x&y=8 x|y=15 x^y=7 ~x=-14 
x>>1=6 x<<1=26 x>>>1=6
```

### 조건 연산자 (삼항 연산자)
```java
import java.util.*;

public class Pizza{
    public static void main(String[] args){
        double area1=2*3.141592*20*20;
        double area2=3.141592*30*30;

        System.out.println("20cm 피자 면적="+area1);
        System.out.println("30cm 피자 면적="+area2);
        System.out.println((area1>area2)? "20cm 두개":"30cm 한 개"); 
    }
}
```
```java
//실행결과
20cm 피자 2개의 면적=2513.2736
30cm 피자 면적=2827.4328
30cm 피자 한 개를 주문하세요.
```
