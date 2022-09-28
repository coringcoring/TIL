# CHAP 05 STUDY

* 아무거나 쓰면서 잠을 깨도록 노력해보자 
* 졸려 

## 객체의 생성과 소멸 
* 자바에서 객체를 생성하는 연산자는 있지만, 삭제하는 연산자는 없음.
* 가비지 컬렉션 garbage collection: 자동 메모리 시스템을 사용 
* 가비지 컬렉터 garbage collector: 히프 메모리에서 더 이상 필요 없는 객체를 찾아 지우는 역할. JVM의 중요한 부분. (대표적으로 오라클사의 HotSpot은 많은 가비지 컬렉션 옵션을 제공.)
* marking -> normal detection -> compaction 

## 가비지 컬렉션 요청 
```java
System.gc();
```
* 단, 가비지 컬렉터가 수행되면 모든 다른 애플리케이션이 멈추기 때문에 가비지 컬렉터의 실행 여부는 JVM이 판단. 

```java
A x  = new A(); //객체 1개 생성 (후에 메모리상에 존재하지만 변수를 통한 참조로 쓸 수 없는 아이가 되어버림..x에 대한 참조가 3번째 줄에서 변경.)
A y = new A();  //객체 새롭게 1개 생성
x = new A();  // x가 새로운 객체 new A()를 참조하도록함. 
```


## 인수 전달 방법
* 자바에서 메소드로 인수가 전달되는 방법은 기본적으로 `값에 의한 호출 call by value` 
* 값을 복사하는 방식. 
```java
int sum= obj.add(25,47);
```

```java
int add(int x, int y){
    return x+y; 
}
```
25가 복사되어 int x=25; 47이 복사되어 int y=47; 처럼 복사되는 것.. 

* 객체를 메소드로 전달하게 되면 객체 자체가 복사되어 전달되는 것이 아니고 객체의 참조값만 복사되어서 전달됨. 참조 변수는 참조값(주소)를 가지고 있음. 
* 참조값이 매개변수로 복사되면 메소드의 매개 변수도 동일한 객체를 참조하게 됨.


* 예제
```java
class Pizza{
    int radius;
    Pizza(int r){
        radius=r;
    }
    Pizza whosLargest(Pizza p){
       if(this.radius>p.radius) return this;
       else return p; 
    }
}

public class PizzaTest{
    public static void main(String[] args){
        Pizza obj1= new Pizza(14);
        Pizza obj2=new Pizza(18); 

        Pizza largest= obj1.whosLargest(obj2);
        System.out.println(largest.radius+"인치 피자가 더 큼"); 
    }
}
```
```java
public class ArrayArgumentTest{
    public static double minArray(double[] list){
        double min=list[0];

        for(int i=1;i<list.length;i++){
            if(list[i]<min>) min=list[i]; 
        }

        return min; 
    }
}
```

## 인스턴스 멤버 vs 정적 멤버 
* 정적 멤버 static member (class memebr): 프로그램을 작성하다보면 여러개의 객체가 하나의 변수를 공유해야 되는 경우가 발생. 
```java
class Television{
    int channel
    int volume;
    boolean onoff;
    static int count; //Television class를 사용하는 객체들만 이를 사용 가능. 
}
```
* 정적 변수 class variable은 클래스당 하나만 생성되는 변수. 앞에다 `static` 붙이면 됨. 
* 정적 변수는 클래스당 딱 하나만 자리를 잡음. 객체마다 새로이 생성되는 것이 아님! 반면에, title과 같은 변수는 각 객체마다 새로이 생성됨. 
* 정적 변수는 여러개일 수 있음. 
```java
class Book{
    String title;
    static int count=0; 
    static int count2=0; 
}

Book a = new Book();
Book b = new Book(); 
Book c = new Book(); 
```

* 예제: 정적 변수 사용하기
```java
public class Pizza{
    private String toppings;
    private int radius;
    static final double PI= 3.141592;
    static int count=0; 

    public Pizza(String toppings){
        this.toppings=toppings;
        count++; 
    }
}

public class PizzaTest{
    public static void main(String[] args){
        Pizza p1=new Pizza("new supreme");
        Pizza p2=new Pizza('cheese');
        Pizza p3=new Pizza("pepperoni");

        int n=Pizza.count; 
        System.out.println("지금까지 판매된 피자 개수 = "+n); 
    }
}
```

## 정적 메소드
* static 수식자를 메소드 앞에 붙임 
* 객체를 생성하지 않고 클래스 이름으로 접근해서 사용 가능

* 예제: 정적 메소드 사용하기
```java
public class MyMath{
    public static int abs(int x){ return x>0?x:-x;}
    public static int power(int base,int exponent){
        int result=1;
        for(int i=1;i<=exponent;i++) result*=base;
        return result; 
    }
}
```
* 정적 메소드는 정적 멤버만 사용 가능함. 
* 정적 메소드에서 정적 메소드를 호출하거나 정적 멤버를 사용하는 것은 가능. 
* 정적 메소드는 this를 사용할 수 없음. (this는 객체를 생성하고나서야 만들어지는 것.)


## final 키워드
* 어떤 필드에 `final`을 붙이면 상수가 됨. 상수를 정의할 때 static과 final 수식어를 동시에 사용하는 경우가 많음

* 예제: 싱글톤 패턴 (하나의 프로그램 내에서 하나의 인스턴스만을 생성해야하는 경우에 사용됨.)
```java
class Single{
    private static Single instance = new Single();
    private Single(){

    }
}
```

## 객체 배열
* 객체를 저장하는 배열
* 객체 배열에는 객체에 대한 참조값이 저장되어 있음 
* 예제: 객체 배열 만들기
```java
import java.util.Scanner;
class Movie{
    String title,director;
    static int count;
    public Movie(String title,String director){
        this.title=title;
        this.director=director;
        count++; 
    }
}
public class MovieArrayTest{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);

        Movie[] list=new Movie[10];
        list[0]=new Movie("백투터퓨처","로버트 저메키스");
        list[1]=new Movie("티파니에서 아침을","에드워드 블레이크"); 


        for(int i=0;i<Movie.count;i++){
            System.out.println("=========================");
            System.out.println("제목: "+list[i].title);
            System.out.println("감독: "+list[i].director);
            System.out.println("=========================");

        }
    }
}
```


## 동적 객체 배열
* 자바의 표준 배열은 크기가 결정되면 변경하기 어려움.
* 예시 1. 
```java
import java.util.ArrayList;

public class ArrayListTest{
    public static void main(String[] args){
        ArrayList<String> list=new ArrayList<String>(); 
        list.add("홍콩");

        list.add("싱가포르");
        list.add("괌");
        list.add("사이판"); 
        list.add("하와이"); 

        System.out.println("여행지 추천 시스템입니다"); 
        int index= (int) (Math.random()*list.size());
        System.out.println("추천 여행지는" + list.get(index)); 
    }
}
``` 
* 예시2: 연락처 정보 저장하기
```java
import java.util.ArrayList;
class Person{
    String name;
    String tel
    public Person(String name,String tel){
        this.name=name;
        this.tel=tel;
    }
}

public class ArrayListTest2{
    public static void main(String[] args){
        ArrayList<Person> list= new ArrayList<Person>();
        list.add(new Person("홍길동", "01012345678"));
        list.add(new Person("김유신", "01012345679"));
        list.add(new Person("최자영", "01012345680"));
        list.add(new Person("김영희", "01012345681"));
        for (Person obj : list)
            System.out.println("(" + obj.name + "," + obj.tel + ")");
    }
}
``` 