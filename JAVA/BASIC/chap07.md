# CHAP 07 STUDY

## 종단 클래스와 종단 메소드 
* `final`이 붙을 수 있는 경우: final 변수(상수),final 메소드(메소드 재지정(상속의 경우에)의 불가한 메소드 지정),final 클래스(상속 불가 클래스)
* 도형 면적 계산하기
```java
class Shape{
    public double getArea(){ return 0; }
    public Shape(){
        super(); 
    }
}

class Rectangle extends Shape{
    private double width,height;
    public Rectangle(double width,double height){
        super();
        this.width=width;
        this.height=height; 
    }
    public double getArea(){ return width*height; }
}

class Triangle extends Shape{
    private double base,height;
    public double getArea(){ return 0.5*base*height; }
    public Triangle(double base,double height){
        super();
        this.base=base;
        this.height=height; 
    }
}
```

## 추상 클래스 abstract class (=미완성 클래스)
* 추상 클래스: `추상 메소드`를 가지고 있는 클래스 
    * `추상 메소드` : 메소드의 선언부만 있는 메소드 (구현이 되어 있지 않은 메소드. 중괄호{}가 없음. 몸체(body)가 없음!) (ex. void sum(); )
    * 메소드가 미완성되어 있으므로 추상 클래스로는 객체를 생성할 수 없음. 
* 주로 `상속 계층`에서 `추상적인 개념`을 나타내기 위한 용도로 사용 
* 정의
```java
public abstract class Animal{
    public abstract void move(); //추상 메소드 정의. ;로 종료됨을 유의 
}

public class Lion extends Animal{
    public void move(){ //추상클래스를 상속받으려면 추상 메소드를 구현해야함.
        System.out.println("사자의 move()메소드 입니다"); 
    }
}
```
* ex> 도형을 나타내는 계층 구조 
```java
abstract class Shape{
    int x,y;
    public void translate(int x,int y){
        this.x=x;
        this.y=y; 
    }
    public abstract void draw(); //추상 메소드 선언. 
}

class Rectangle extends Shape{
    int width,height; 
    public void draw(){ System.out.println("사각형 그리기 메소드"); }
    //자식 클래스에서 추상 메소드를 구현하지 않으면 컴파일 오류가 발생함. 
    //만약 여기서 추상 메소드를 구현하고 싶지 않다? : class abstract Rectangle extends Shape로 선언해야함. 이 Rectangle 클래스를 상속받은 자식 클래스에서 추상 메소드를 정의해주어야.. 
}

class Circle extends Shape{
    int radius;
    public void draw(){ System.out.println("원 그리기 메소드");}
}

public class AbstractTest{
    public static void main(String[] args){
        Shape s1=new Shape(); //오류! 추상 클래스로 객체를 생성할 수 없음
        Shape s2=new Circle();
        s2.draw(); 
    }
}
```
* 추상 클래스의 용도 
    * 추상 메소드로 정의되면 자식 클래스에서 반드시 오버라이드해야함. 하지 않으면 오류 발생. 
    * 일반 메소드로 저의되면 자식 클래스에서 오버라이드하지 않아도 컴파일러가 체크할 방법이 x

## 인터페이스
* 하드웨어 인터페이스: 서로 다른 장치들이 연결되어서 상호 데이터를 주고받는 규격을 의미. 
* 인터페이스의 용도
    * 상속관계가 아닌, `클래스 간의 유사성을 인코딩`하는데 사용 
* 정의: 인터페이스는 추상 메소드들과 디폴트 메소드들로 이루어짐. 인터페이스 안에서 필드(변수)는 선언될 수 없음. 상수는 정의 가능. 
```java
public interface RemoteControl{
    public void turnOn();
    public void turnOff(); 
}
```
* 구현: 다른 클래스에 의해 implement 될 수 있음. 인터페이스에 정의된 추상 메소드의 몸체를 정의.
```java
class Television implements RemoteControl{
    boolean on; 
    public void turnOn(){
        on=true; 
        System.out.println("tv가 켜졌습니다"); 
    }
    /**/
}
```
* 인터페이스 vs 추상클래스 
    * 추상 클래스 사용 권장 
        * 관련된 클래스 사이에서 코드를 공유하고 싶다면 추상 클래스 사용
        * 공통적인 필드나 메소드가 많은 경우 또는 public 이외의 접근 지정자를 사용해야하는 경우 추상 클래스 사용
        * 일반 멤버 필드를 선언하기 원할 때 사용 
    * 인터페이스 사용 권장
        * 관련없는 클래스들을 동일한 도작을 구현하기 원할 때 사용. 
        * 특정한 자료형의 동작을 지정하고 싶지만 누가 구현하든지 신경 쓸 필요가 없을 때 사용
        * 다중 상속이 필요할 때 
* 인터페이스를 정의하는 것은 새로운 자료형을 정의하는 것과 마찬가지. -> `다형성` 지원 가능. 
* 예제: 원격제어 인터페이스
```java
interface RemoteControl{
    void turnOn();
    void turnOff(); 
    public default void printBrand(){   System.out.println("Remote control 가능 tv"); }
}
/***/
```
* 인터페이스들끼리도 `상속` 가능.
* 인터페이스를 이용한 다중 상속 
    * `다중 상속`: 하나의 클래스가 여러 개의 부모 클래스를 가지는 것. 
    * 문제점: 애매모호한 상황을 만들 수 있기 때문에 자바에서는 금지. 
    * 1. 동시에 여러개의 인터페이스를 구현하면 다중 상속의 효과를 낼 수 있음. 
    ```java
    interface Drivable {    void Drive(); }
    interface Flyable(      void fly();)
    ```
    * 2. 하나의 클래스를 상속받고 또 하나의 인터페이스룰 구현 하는 방법

* 인터페이스에서의 상수 정의: `public static final`
```java
interface Test{
    int a; //불가능. 자동 상수 선언인데 상수값은 초기값이 필수이므로 안됨. 
}
```
* 다중 상속 예제
```java 
class Shape{
    protected int x,y; 
}
interface Drawable{
    void draw(); 
}
class Circle extends Shape implements Drawable{
    int radius; 
    public void draw(){
        System.out.println("Circle Draw")); 
    }
}

public class TestInterface2{
    public static void main(String[] args){
        Drawable obj=new Circle();
        obj.draw(); 
    }
}
```

## 디폴트 메소드와 정적 메소드 
* Java7 이전 버전까지는 인터페이스 안에 추상 메소드와 상수만 허용
* Java8에서 디폴트 메소드와 정적 메소드가 추가됨. Java9에서는 전용 메소드까지 추가됨. 

* `디폴트 메소드`: 인터페이스 개발자가 메소드의 디폴트 구현을 제공하 수 있는 기능. 
```java
interface Drawable{
    void draw();
    default void getSize(){} //굳이 클래스마다 구현해줄 필요 없음. 
    //다시 정의할 수 있지만 기본 구현을 그대로 사용해도 됨. 
}
```

* `정적 메소드` : Java8 이전에는 인터페이스에 딸린 정적 메소드를 제공하려면 인터페이스와는 별도의 유틸리티 크래스와 헬퍼 메소드를 작성해야 했음. 
``` 
```

## 인터페이스와 팩토리 메소드 
* 최근에 인터페이스에서도 `팩토리 메소드`가 있는 것이 좋다고 간주되고 있음.
* 공장처럼 객체를 생성하는 정적 메소드. 


```java
Rectangle r[]=new Rectangle[10]; 
//10개 객체 사각형 생성
//반복문 돌면셔...r[i]=new Rectangle(..); 

Arrays.sort(r); //앞에서의 Comparable 구현 필요. 
```

## 중첩 클래스 
* 클래스 안에서 클래스를 정의. 
* `외부 클래스`, `내부 클래스` 
* 분류
    * 정적 중첩 클래스: 앞에 `static`이 붙어서 내장되는 클래스.
    * 비정적 중첩 클래스: `static`이 붙지 않은 일반적인 중첩 클래스 
        * 내부 클래스
        * 지역 클래스
        * 익명 클래스 
* 사용하는 이유: 내부 클래스는 외부 클래스의 private 멤버도 접근 가능. 

## 익명 클래스
* 클래스 몸체는 정의되지만 이름이 없는 클래스. 클래스를 정의하면서 동시에 객체를 생성. 이름이 없기 때문에 한 번만 사용 가능. 
* 객체 생성을 함과 동시에 몸체 정의 
