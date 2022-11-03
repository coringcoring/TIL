# CHAP 06 STUDY

## 상속 Inheritance
* 부모클래스에 정의된 멤버변수, 메소드를 자식 클래스가 물려받음.
* `extends` : 부모 클래스를 확장하여 자식 클래스를 작성 

```java
class Car{
    int speed;
    public void setSpeed(int Speed){
        this.speed=speed; 
    }
}

public class ElectricCar extends Car{
    int battery;

    public void charge(int amount){
        battery+=amount; 
    }
}

public class ElectricCarTest{
    public static void main(String[] args){
        ElectricCar obj=new ElectricCar(); 

        obj.speed=10;
        obj.setSpeed(60);
        obj.charge(10); 
    }
}
```
* `다중 상속` (여러 개의 클래스로부터 상속받는 것)지원 x 
    * 클래스 간의 다중 상속은 지원하지 않음! (c++은 지원함.)
* 상속의 횟수에는 제한 x
* 상속 계층 구조의 최상위에는 java.lang.Object 클래스가 존재. 

```java
class Animal{
    int age;
    void eat(){
        System.out.println("먹고 있음.."); 
    }
}

class Dog extends Animal{
    void bark(){
        System.out.println("짖고 있음..."); 
    }
}

public class DogTest{
    public static void main(String[] args){
        Dog d = new Dog();
        d.bark();
        d.eat(); 
    }
}
```

```java
class Shape{
    int x,y;
}

class Circle extends Shape{
    int raidus; 
    public Circle(int radius){
        this.radius=radius;
        x=0;
        y=0; 
    }

    double getArea(){
        return 3.14*radius*radius; 
    }
}
```

## 상속과 접근 지정자 
* public, protetcted, private, default
```java
class class Shape{
    protected int x,y;
    void print(){
        System.out.println("x좌표: "+x+"y좌표:"+y); 
    }
}
```


## 상속과 생성자
* 자식 클래스의 생성자만 호출될까? : 아니다
* 부모 클래스의 생성자도 호출되는가? 맞다. 
* 어떤 순서로 호출될까? : 부모 -> 자식 순 


```java
class Base{
    public Base(){
        super(); 
        System.out.println("base()");
    }
}

class Derived extends Base{
    public Derived(){
        super(); //부모 클래스 생성자 호출 
        System.out.println("derived()");
    }
}

public class Test{
    //...new Derived(); 
}
``` 

* 부모 클래스의 생성자 -> 자식 클래스의 생성자 순서. 
* 오류가 발생하는 경우: 묵시적인 부모 클래스 생성자 호출을 사용하려면 부모 클래스에 기본 생성자(매개변수가 없는 생성자)가 반드시 정의되어 있어야함. 

* Person 클래스와 Employee
```java
class Person{
    String name;
    public Person(){}
    public Person(String theName){
        this.name=theName; 
    }
}

class Employee extends Person{
    String id; 
    public Employee(){ super(); }
    public Employee(String name){ super(name); }
    public Employee(String name,String id){
        super(name);
        this.id=id; 
    }
    @Override
    public String toString(){
        return "Employee [id= "+id+" name="+name+"]"; 
    }
}
```

## 메소드 오버라이딩 Method Overriding 
* 자식 클래스가 부모 클래스의 메소드를 자신의 필요에 맞추어 재정의하는 것 
* 메소드의 이름, 매개변수, 반환형은 동일해야함. 


