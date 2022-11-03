# CHAP 08 STUDY

## 패키지 Package
* 관련있는 클래스들을 하나로 묶은 것 
* 종류
    * 내장 패키지: 자바에서 기본적으로 제공하는 패키지들
    * 사용자 정의 패키지: 사용자가 정의하는 패키지들 
* 필요한 이유
    * 패키지를 이용하면 서로 관련된 클래스들을 하나의 단위로 모을 수 있음 
    * `이름 공간(name space)` : 동일한 이름의 클래스가 각 다른 패키지에 속할 수 있으므로 `이름 충돌`을 방지 가능 
    * 패키지를 이용하여 세밀한 접근 제어를 구현 가능. 패키지 안의 클래스들은 패키지 안에서만 사용이 가능하도록 선언될 수 있음. 
* 선언하기
    ```java
    package graphics;  
    
    public class Circle{
        double radius; 
    }
    ```
* 사용하기 
    * 완전한 이름(패키지 이름이 클래스 앞에 붙은 것)으로 참조
        ```java
        graphics.Rectangle myRect= new graphics.Rectanlge();
        ```
    * 패키지 안에서 우리가 원하는 클래스만을 포함 
        ```java
        import graphics.Rectangle;
        Rectangle myRect = new Rectangle();
        ```
    * 패키지 안의 모든 클래스를 포함 
        ```java
        import graphics.*;
        Rectangle myRect = new Rectangle();
        ``` 
* 계층 구조의 패키지 포함하기 
    ```java
    import java.awt.*; //java.awt.font 패키지는 자동으로 포함x 
    import java.awt.font.*; //별도로 포함시켜야함. 
    ```

## 클래스 파일은 언제 로드되나?
* 자바 소스 파일(.java)가 컴파일되면 .class의 클래스 파일로 변환되어 파일 시스템에 저장됨. 클래스 파일은 JVM(자바 가상 기계)에 의해서 로드됨 
    * 클래스 파일은 `요청 시 JVM에 로드`됨. 기본적인 방법. `지연 클래스 로드(lazy class loading)`이라고도 함.
    * 애플리케이션 코드를 구성하는 기본적인 클래스 파일은 시작시 로드됨  
* 자바 가상 기계가 클래스 찾는 순서 
    * 부트스트랩 클래스
        * 자바 플랫폼을 구성하는 핵심적인 클래스
        * 디렉터리 jre/lib에 있는 rt.jar을 포함한 jar 파일들
        * JAVA 9 이후부터는 jmods 파일이 로드됨
    * 확장 클래스
        * 자바 확장 메커니즘을 사용하는 클래스들
        * 확장 디렉터리에 있는 jar 파일들
        * jre/lib/ext에 있는 모든 jar 파일들을 확장으로 간주되어 로드됨 
    * 사용자 
        * 확장 메커니즘을 사용하지 않는 개발자 및 타사에서 정의한 클래스들 
        * 가상 기계 명령줄에서 -classpath 옵션을 사용하거나 CLASSPATH 환경변수 사용하여 클래스의 위치를 식별 
* 자바 가상 기계가 클래스 찾는 3가지 방법 
    * 현재 디렉터리에서부터 찾음
    * 일반적으로 환경 변수인 `CLASSPATH`에 설정된 디렉터리에서 찾음 
        * CLASSPATH 변수를 설정하려면 명령 프롬프트에서 명령어 사용하여 지정 
    * 또는 가상 머신을 실행할때 옵션 -classpath를 사용한다. 즉, 가상머신을 실행할때 클래스 경로를 알려주는 것. 이클립스는 내부적으로 이 방법을 사용하고 가장 권장되는 방법임 
        * 클래스 경로 CLASSPATH: 사용자가 직접 작성했거나 외부에서 다운받은 클래스들을 찾기 위하여 자바 가상 머신이 살펴보는 디렉터리들을 모아둔 경로. 

## JAR 압축 파일 
* Java Archive: 자바 파일들을 압축하여 하나의 파일로 만드는데 사용 
* 만드는 방법 
    * JDK 안에 포함된 jar 도구를 이용하여 jar 파일 생성 
        C:\ jar cvf Game.jar *.class icon.png
    * 실행 가능한 jar 파일 만들려면 e 옵션 추가
        C:\ jar cvfe Game.jar *.class icon.png
    * jar 파일로 압축된 파일을 실행하는 방법 
        C:\ java -jar Game.jar 
* jar 형태의 라이브러리 사용
    * 외부로부터 받은 jar 파일 graphics.jar을 사용하려면 클래스 경로에 graphics.jar 파일을 포함시키면 됨 
        C:\ set CLASSPATH=C:\classes;C:\lib;C\graphis.jar;. 

## 자바 API 패키지 
* java.awt: 이미지, 그래픽등을 지원하기 위한 패키지
* javax.swing: 스윙 컴포넌트를 관리하는 패키지
* java.lang: 자바 프로그래밍 언어에 필수적인 패키지
* java.util: 날짜, 난수 생성기 등의 유틸리티 패키지 

## Object 클래스 
* java.lang에 있음
* 클래스 계층 구조에서 가장 최상위에 있는 클래스 
* public boolean equals(Object obj) : obj가 이 객체와 같은지를 검사 
    * ==을 사용하여 객체 주소가 똑같은지를 검사(true or false 반환). 그러나 객체에 대해서 이가 맞지 않는 경우가 많음 
    ```java
    class Circle{
        int radius;
        public Circle(int radius){ this.radius= radius; }
        public boolean equals(Object o){
            Circle c1= (Circle) o; 
            if (radius ==c1.radius) return true; 
            else return false; 
        }
    }

    public class CircleTest{
        public static void main(String[] args){
            Circle obj1 = new Circle(10);
            Circle obj2 = new Circle(10);
            if (obj1.equals(obj2)) System.out.println("2개의 원은 같음");
            else System.out.println("2개의 원은 같지 않습니다"); 
        }
    }
    ``` 
* public String toString(): 객체의 문자열 표현을 반환
* public int hashCodes(): 객체의 해시코드를 반환 
* protected Object clone(): 객체 자신의 복사본을 생성하여 반환 
* protected void finalize(): 가비지 콜렉터에 의하여 호출됨
* public final Class getClass(): 객체의 클래스 정보를 반환 
    ```java
    class Circle{}
    public class CircleTest{
        public static void main(String[] args){
            Circle obj = new Circle();
            System.out.println("obj is of type "+obj.getClass().getName()); //obj is of type test.Circle
            System.out.println("obj의 해쉬코드: "+ obj.hashCode()); //obj의 해쉬코드: 1554547125 
        }
    }
    ```

## 랩퍼 클래스 (Wrapper Class)
* 정수와 같은 기초 자료형도 객체로 포장하고 싶을때 사용 
    ```java
    int i=100;
    Integer obj = new Integer(i);
    ```
* 메소드 
    * intValue(): int형으로 반환
    * doubleValue(): double형으로 반환
    * floatValue(): float형으로 반환
    * parseInt(String S): 문자열을 int형으로 반환
    * toBinaryString(int i): int형의 정수를 2진수 형태의 문자열로
    * toString(int i): int형의 정수를 10진수 형태의 문자열로 
    * valueOf(String s): 문자열 s를 Integer 객체로 변환 
    * valueOf(String s, in radix): 문자열 s를 radix진법의 integer 객체로 변환 
* 예시
    * 랩퍼 객체 안에 있는 100을 꺼내고 싶을때 intValue()사용 (사실 오토박싱 더 많이 실사용)
        ```java
        Integer obj= new Integer(100);
        int value= obj.intValue();
        ```
    * 정수 100을 문자열로 변환하고자 한다면 
        ```java
        String s= Integer.toString(100); //"100"으로 반환 
        ```
    * 문자열 "100"을 정수 100으로 변환 
        ```java
        int i= Integer.parseInt("100");
        ``` 
    
## 오토 박싱
* 랩퍼 객체와 기초 자료형 사이의 변환을 자동으로 해주는 기능
    ```java
    Integer obj
    obj= 10; //정수 -> Integer 객체 
    System.out.println(obj+1); //Integer 객체 -> 정수로 자동 변환 
    ```

## String 클래스
```java
String s1="java"; //s1과 s2는 같은 string을 가리키고 있음 
String s2="java"; //s1==s2하면 true로 나옴. s1.equals(s2)도 마찬가지. 
String s3= new String("java"); //원래 원칙대로 사용하면 이거여야함.
String s4=new String("java"); //s3==s4 false로 나옴. s3.equals(s4)는 true. 
```
* 기초 연산
    * length():문자열의 길이 반환
    * charAt(): 문자 추출. 인덱스로 
    * concat(): 2개의 문자열을 붙임 그러나 + 연산자로 붙이는게 훨 나음. 

* 문자열 안에서 단어 찾기 
```java
String s= "java is awesome"; 
int index= s.indexOf("is");

if(index==-1) System.out.println("is는 없음.");
else System.out.println("is의 위치: "+index); // is의 위치: 5 
```
* 문자열을 단어로 분리 
    * 예전에는 StringTokenizer 클래스 사용 
    * 지금은 String 클래스가 제공하는 split()메소드 사용 편리 
        ```java
        String s="10 20 30 40 50"; 
        String[] tokens= s.split(" ");
        int sum=0; 

        for (int i=0;i<tokens.length();i++){
            sum+=Integer.parseInt(tokens[i]); 
            System.out.println(tokens[i]); 
        }
        ```
* String은 한번 설정하면 변경 불가능함. 참조변수가 대상을 바꿀 수는 있지만 메모리에 있는 그 내용 자체를 변경 불가능. 
    ```java
    String s1="Hello World!";
    String s2="Java Test  "; //s2를 trim하면 뒤에 공백은 삭제, 중간에 공백은 삭제x 

    String s3=s1.replace('o','O'); 
    System.out.println(s3); //HellO WOrld! 
    System.out.println(s1); //Hello world! <-변경되지 않음 

    String s4=s1.substring(6); //6인덱스로부터 추출 
    System.out.println(s1); //안바뀜. Hello World!
    System.out.println(s4); //World! 
    ``` 

## StringBuffer 클래스 
* String 클래스의 경우 빈번하게 문자열의 내용을 바꿔야하는 경우 비효율적 
    * 왜: 문자열의 내용을 바꿀때 String 클래스 메소드는 새로운 객체를 생성하고 기존의 내용을 복사해야하기 때문 


