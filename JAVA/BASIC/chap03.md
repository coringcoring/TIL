# CHAP 03 STUDY 

## if-else 문 
* 조건에 따라서 서로 다른 처리를 하고 싶을 때 사용하는 구조

```java
//짝수와 홀수 구별하기
import java.util.Scanner;

public class EvenOdd{
    public static void main(String[] args){
        int number;
        Scanner sc=new Scanner(System.in);

        System.out.print("정수를 입력하시오: ");
        number=sc.nextInt();

        if(number%2==0){
            System.out.println("입력된 정수는 짝수입니다.");
        }else{
            System.out.println("입력된 정수는 홀수입니다.");
        }
    }
}
```

* 다중 if-else문 
```java
import java.util.Scanner;

public class Nested{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        System.out.print("정수를 입력하시오: ");
        int number=sc.nextInt();

        if(number>0)
            System.out.println("양수입니다.");
        else if (number==0)
            Sysetm.out.println("0입니다.");
        else
            System.out.println("음수입니다."); 
    }
}
```


```java
//학점 결정 
import java.util.Scanner;

public class Grading{
    public static void main(String[] args){
        int score; 
        Scanner sc=new Scanner(System.in);
        System.out.print("성적을 입력하시오: ");
        score=sc.nextInt(); 

        if(score>=90){
            System.out.println("학점 A");
        }
        else if(score>=80){
            System.out.println("학점 B");
        }
        else if(score>=70){
            System.out.println("학점 C");
        }
        else if(score>=60){
            System.out.println("학점 D"); 
        }
        else{
            System.out.println("학점 F"); 
        }
    }
}
```

```java
//가위 바위 보 게임 
import java.util.*; 

public class RockPaperScissor{
    final int SCISSOR=0;
    final int ROCK=1;
    final int PAPER=2; 

    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        System.out.print("가위(0) 바위(1) 보(2)");
        int user=sc.nextInt();

        int computer=(int)(Math.random()*3); 
        
        if(user==computer){
            System.out.println("인간과 컴퓨터가 비겼음");
        }
        else if (user==(computer+1)%3){
            System.out.println("인간: "+user+"컴퓨터: "+computer+" 인간 승리");
        }
        else{
            System.out.println("인간: "+user+"컴퓨터: "+computer+" 컴퓨터 승리"); 
        }
    }
}
```

## switch 문 
```java
//학점 결정
import java.util.*;

public class Score2Grade{
    public static void main(String[] args){
        int score,number;
        char grade;

        Scanner sc=new Scanner(System.in);
        System.out.print("성적을 입력하시오: ");
        score=sc.nextInt();

        number=score/10;
        switch(number){
            case 10:
            case 9: grade='A'; break;
            case 8: grade='B'; break;
            case 7: grade='C'; break;
            case 6: grade='D'; break;
            default: grade='F'; break;
        }
        System.out.print("학점: "+grade); 
    }
}
```
* Java7 부터는 switch 문의 제어식으로 String 객체를 사용 가능. 

```java
//피자 종류를 입력 받아서 피자의 가격을 반환하는 프로그램 
import java.util.Scanner;

public class StringSwitch{
    public static void main(String[] args){
        Scanner sc= new Scanner(System.in);
        System.out.print("피자의 종류를 입력하시오: ");

        String model=sc.next();
        int price=0;

        switch(model){
            case "콤비네이션": 
            case "슈퍼슈프림": price=20000; break;
            case "포테이토": price=15000; break;
            case "쉬림프": price=25000; break;
            default: price=0 break; 
        }
        System.out.println("피자"+model+"의 가격="+price); 
    }
}
```


* 향상된 swtich 문 
    * Java 12부터는 화살표를 사용하는 향상된 switch문을 사용 가능. 
```java
public class Test{
    public static void main(String[] args){
        var day="SAT";
        var today="";
        switch(day){
            case "SAT","SUN"->today="주말";
            case "MON","TUS","WED","THU","FRI"-> today="주중";
            default -> System.out.println("Error"); 
        }
        System.out.println(today); 
    }
}
```

## for문 
```java
//0부터 4까지 출력하기
public class ForExample1{
    public static void main(String[] args){
        for(int i=0;i<5;i++){
            System.out.println("i의 값은: "+i); 
        }
    } 
}

//정수의 합 계산하기 
public class Sum{
    public static void main(String[] args){
        int sum=0;
        
        for(int i=1;i<=10;i++){
            sum+=i; 
        }

        System.out.print("1부터 10까지의 정수의 하 %d\n",sum); 
    }
}

//팩토리얼 계산하기 
import java.util.*;

public class Factorial{
    public static void main(String[] args){
        long fact=1; 
        int n;

        System.out.printf("정수를 입력하시오: ");

        Scanner scan=new Scanner(System.in); 
        n=scan.nextInt();

        for(int i=1;i<=n;i++){
            fact=fact*i; 
        }
        
        System.out.printf("%d!는 %d입니다\n",n,fact); 
    }
}

//약수 계산하기
import java.util.*;

public class Divisor{
    public static void main(String[] args){
        Scanner scan=new Scanner(System.in);
        System.out.print("양의 정수를 입력하시오: ");

        int n=scan.nextInt();

        System.out.println(n+"의 약수는 다음과 같습니다."); 
        for (int i=1;i<=n;++i){
            if(n%i==0)
                System.out.print(" "+i); 
        }
    }
}
```

## while 문 
```java
public class WelcomeLoop{
    public static void main(String[] args){
        int i=0;
        while (i<5){
            System.out.println("환영합니다!");
            i++; 
        }
        System.out.println("반복이 종료되었습니다."); 
    }
}

//-1의 값이 입력될 때까지 합계 계산하기
import java.util.Scanner;

public class GetSum{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);

        int sum=0,value=0; 
        
        while(value!=-1){
            sum=sum+value; 
            System.out.print("정수를 입력하시오: "); 
            value=sc.nextInt(); 
        }
        System.out.println("정수의 합은"+sum+"입니다."); 
    }
}
```

## do-while문 
```java
//정확한 입력 받기 
import java.util.Scanner;

public class CheckInput{
    public staic void main(String[] args){
        Scanner sc=new Scanner(System.in); 
        int month;

        do{
            System.out.print("올바른 월을 입력하시오 [1-12]: ");
            month=sc.nextInt();
        }while(month<1||month>12); 

        System.out.println("사용자가 입력한 월은 "+month); 
    }
}
```

## 중첩 반복문 
* 반복문은 중첩되어 사용될 수 있다. 즉 반복문 안에 다른 반복문이 실행될 수 있다.
* 이러한 형태를 `중첩 반복문(nested loop)`이라고 한다. 

```java
//사각형 모양 출력하기 
import java.util.*;
public class NestedLoop{
    public static void main(String[] args){
        for(int y=0;y<5;y++){
            for(int x=0;x<10;x++)
                System.out.print("*"); 
            System.out.println(""); 
        }
    }
}

//삼각형 모양 출력하기
public class PrintTriangle{
    public static void main(String[] args){
        for(int y=0;y<8;y++){
            for(int x=0;x<y+1;x++){
                System.out.print("*");
            }
            System.out.println(); 
        }
    }
}
```

## 무한루프 
* while문을 사용할 때, 종료 조건을 만들려면 까다로운 경우 있음.
* 이러한 경우 while(true)를 이용하여 무한 루프를 만들고 무한 루프 안에서 break를 사용해서 루프를 빠져나가는 조건을 기술하는 편이 가독성 높고 코딩하기 쉬움. 

```java
import java.util.*;
public class Averager {
    public static void main(String[] args) {
        int total = 0, count = 0;
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.print("점수를 입력하시오: ");
            int grade = sc.nextInt();
            if (grade < 0)
                break;
            total += grade;
            count++;
        }
        System.out.println("평균은 " + total / count);
    }
}
```

## 배열 array
* 여러 개의 변수를 하나로 묶어 넣은 것. 배열을 사용하면 같은 종류이 대량의 데이터를 한 번에 선언할 수 있다. 

```java
int[] s= new int[10]; 
```

## 배열의 선언과 사용 
* 배열의 선언 (참조 변수 선언)
* 배열의 생성 -> new 연산자를 사용하여 생성. 

```java
//배열의 각 요소는 index로 접근 
public class ArrayTest1{
    public static void main(String[] args){
        int[] s=new int[10]; 
        for(int i=0;i<s.length;i++){
            s[i]=i; 
        }
        for(int i=0;i<s.length;i++){
            System.out.print(s[i]+" "); 
        }
    }
}

//배열의 초기화 
public class ArrayTest3{
    public static void main(String[] args){
        int[] scores={10,20,30,40,50};

        for(int i=0;i<scores.length;i++){
            System.out.print(scores[i]+" "); 
        }
    }
}
```

## for-each 루프 
```java
public class ArrayTest4{
    public static void main(String[] args){
        int[] numbers={10,20,30}; 

        for(int value:numbers)
            System.out.print(value+" "); 
    }
}

public class PizzaTopping{
    public static void main(String[] args){
        String[] toppings={"Pepperoni","Mushrooms","Onions","Saussage","Bacon"};

        for(String s:toppings){
            System.out.print(s+" "); 
        }
    }
}
``` 

## 2차원 배열 
```java
//극장 관객 수 계산
public class TheaterSeats{
    public static void main(String[] args){
        int [][] seats={
            {0, 0, 0, 1, 1, 0, 0, 0, 0, 0},
            {0, 0, 1, 1, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 1, 1, 1, 0}
        }; 
        int count=0; 

        for(int i=0;i<seats.length;i++)
            for(int k=0;k<seats[i].length;k++)
                count+=seats[i][k]; 

        System.out.print("현재 관객 수는 "+count+"명입니다."); 
    }
}
```

## 래그드 배열 
```java
int [][] ragged= new int[MAX_ROWS+1][]; 

for (int r=0;r<=MAX_ROWS;r++){
    ragged[r]=new int[r+1]; 
}
``` 

```java
public class RaggedArray{
    public static void main(String[] args){
        int [][] ragged=new int[3][];
        ragged[0]=new int[1];
        ragged[1]=new int[2];
        ragged[2]=new int[3]; 
        
        for(int r=0;r<ragged.length;r++){
            for(int c=0;c<ragged[r].length;c++){
                ragged[r][c]=c; 
            }
        }
    }
}
```

```java
//래그드 배열 생성
import java.util.Arrays;

public class RaggedArray2{
    public static void main(String[] args){
        int[][] rarray= new int[3][]; 

        rarray[0]=new int[] {1,2,3,4}; 
        rarray[1]=new int[] {5,6,7};
        rarray[2]=new int[] {8,9}; 

        /*
        int [][]rarray={
            {1,2,3},{4,5,6,7},{8,9}
        }; 로 대체 가능. 
        */

        for(int[]row:rarray){
            System.out.println(Arrays.toString(row)); 
        }
    }
}
```

## ArrayList
* 배열의 크기를 동적으로 변경하면서 사용 가능. 

```java
import java.util.*;

public class ArrayListTest{
    public static void main(String[] args){
        
        ArrayList<String> list=new ArrayList<>();

        list.add("철수");
        list.add("영희");
        list.add("순신"); 
        list.add("자영");
        for(String obj:list){
            System.out.print(obj+" "); 
        }
    }
}
``` 