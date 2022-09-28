# CHAP 04 STUDY 

## 클래스의 작성 
* 클래스는 객체를 만들기 위한 설계도(틀). 
* 실제로 어떤 작업을 하려면 객체를 생성하여야 한다. 

```java
public class CircleTest{
    public static void main(String[] args){
        Circle obj; 

        obj=new Circle(); 
        obj.radius=100; 
        obj.color="blue"; 

        double area= obj.getArea(); 
        System.out.println("원의 면적="+area); 
    }
}
```

## 예제 
* 데스크 램프 클래스
```java
public class DeskLamp{
    //인스턴스 변수 정의
    private boolean isOn; //켜짐이나 꺼짐과 같은 램프의 상태 

    //메소드 정의
    public void turnOn(){
        isOn=true; 
    }
    public void turnOff(){
        isOn=false;
    }
    public String toString(){
        return "현재 상태는 "+(isOn==true?"켜짐":"꺼짐"); 
    }
}


public class DeskLampTest{
    public static void main(String[] args){
        DeskLamp myLamp = new DeskLamp(); 

        myLamp.turnOn();
        System.out.println(myLamp); 
        myLamp.turnOff();
        System.out.println(myLamp); 
    }
}
```


* 박스 
```java
class Box {
    int width;
    int length;
    int height;
    double getVolume() {
        return (double) width*height*length; 
    }
}

public class BoxTest {
    public static void main(String[] args) {
        Box b;
        b = new Box(); 
        b.width = 20;
        b.length = 20;
        b.height = 30;
        System.out.println("상자의 가로, 세로, 높이는 " + b.width + ", " + b.length+", " + b.height + "입니다.");
        System.out.println("상자의 부피는 " + b.getVolume() + "입니다.");
    }
}
``` 

* Television
```java
public class Television {
    int channel; // 채널 번호
    int volume; // 볼륨
    boolean onOff; // 전원 상태
    public static void main(String[] args) {
        Television myTv = new Television(); 
        myTv.channel = 7;
        myTv.volume = 10;
        myTv.onOff = true;

        Television yourTv = new Television(); 
        yourTv.channel = 9;
        yourTv.volume = 12;
        yourTv.onOff = true;

        System.out.println("나의 텔레비전의 채널은 " + myTv.channel + "이고 볼륨은 " + myTv.volume + "입니다.");
        System.out.println("너의 텔레비전의 채널은 " + yourTv.channel + "이고 볼륨은 " + yourTv.volume + "입니다.");
    }
}
```

## 메소드 오버로딩 Method Overloading (중복정의)
* 이름이 동일한 여러 개의 메소드 작성 가능 
* 메소드 이름은 동일하면서, 매개변수 수, 타입, 순서가 달라야함. 

```java
public class MyMath {
    int add(int x, int y) { return x+y; }
    int add(int x, int y, int z) { return x+y+z; }
    int add(int x, int y, int z, int w) { return x+y+z+w; }

    public static void main(String[] args) {
        MyMath obj;
        obj = new MyMath();
        System.out.print(obj.add(10, 20)+" ");
        System.out.print(obj.add(10, 20, 30)+" ");
        System.out.print(obj.add(10, 20, 30, 40)+" ");
    }
}
```


## 생성자 constructor
* 클래스 이름과 동일하면서 반환값이 없는 메소드
* 중복 정의 가능
* 객체가 생성될 때 객체를 초기화하는 특수한 메소드 

```java
class Pizza {
    int size;
    String type;
    public Pizza() {
        size = 12;
        type = "슈퍼슈프림";
    }
    public Pizza(int s, String t) {
        size = s;
        type = t;
    }
}

public class PizzaTest {
    public static void main(String[] args) {
        Pizza obj1 = new Pizza();
        System.out.println("("+obj1.type+ " , "+obj1.size+",)");

        Pizza obj2 = new Pizza(24, "포테이토");
        System.out.println("("+obj2.type+ " , "+obj2.size+",)");
    }
}
``` 

```java
class Television {
    private int channel; // 채널 번호
    private int volume; // 볼륨
    private boolean onOff; // 전원 상태

    Television(int c, int v, boolean o) {
        channel = c;
        volume = v;
        onOff = o;
    }
    void print() {
        System.out.println("채널은 " + channel + "이고 볼륨은 " + volume + "입니다.");
    }
}

public class TelevisionTest {
    public static void main(String[] args) {
        Television myTv = new Television(7, 10, true);
        myTv.print();

        Television yourTv = new Television(11, 20, true);
        yourTv.print();
    }
}
```

## 기본 생성자 default constructor
* 매개 변수가 없는 생성자 
* 개발자가 생성자를 하나도 정의하지 않으면 자바 컴파일러는 기본 생성자를 자동으로 만듦.

```java
class Box {
    int width, height, depth;
}
public class BoxTest {
    public static void main(String[] args) {
        Box b = new Box();
        System.out.println("상자의 크기: (" + b.width + "," + b.height + "," + b.depth + ")");
    }
}
```

* 추가되지 않는 경우 : 프로그래머가 생성자를 한 개 이상 정의한 경우 
```java
public class Box {
    int width, height, depth;

    public Box(int w, int h, int d) { 
        width=w; height=h; depth=d; 
    }

    public static void main(String[] args) {
        Box b = new Box(); // 오류 발생!!
        System.out.println("상자의 크기: (" + b.width + "," + b.height + "," + b.depth + ")");
    }
}
``` 

## 접근자 getters 와 설정자 setters 메소드 
* 설정자에서 매개 변수를 통하여 잘못된 값이 넘어오는 경우, 이를 사전에 차단 가능.
* 필요할 때마다 필드값을 동적으로 계산하여 반환 가능
* 접근자만을 제공하면 자동적으로 읽기만 가능한 필드 만들 수 있음. 

```java
class Account {
    private int regNumber;
    private String name;
    private int balance;
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getBalance() { return balance; }
    public void setBalance(int balance) { this.balance = balance; }
}

public class AccountTest {
    public static void main(String[] args) {
        Account obj = new Account();
        obj.setName("Tom");
        obj.setBalance(100000);
        System.out.println("이름은 " + obj.getName() + " 통장 잔고는 "+ obj.getBalance() + "입니다.");
    }
}
```

```java
class SafeArray {
    private int a[];
    public int length;
    public SafeArray(int size) {
        a = new int[size];
        length = size;
    }
    public int get(int index) {
        if (index >= 0 && index < length) {
            return a[index];
        }
        return -1;
    }
    public void put(int index, int value) {
        if (index >= 0 && index < length)
            a[index] = value;
        else
            System.out.println("잘못된 인덱스 " + index);
    }
}

public class SafeArrayTest {
    public static void main(String args[]) {
    SafeArray array = new SafeArray(3);
    for (int i = 0; i < (array.length + 1); i++) {
            array.put(i, i * 10);
        }
    }
}
```


