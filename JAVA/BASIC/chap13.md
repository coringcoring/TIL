# CHAP 13 STUDY

## 제네릭이란
* `제네릭 프로그래밍`: 다양한 종류의 데이터를 처리할 수 있는 클래스와 메소드를 작성하는 기법 
* 기존 방법
    * 일반적인 객체 처리: Object 참조 변수(어떤 객체든지 참조 가능) 사용
* 제네릭을 이용한 방법 
     ```java
     class Box<T>{ //T는 타입을 의미 
        private T data; 
        public void set(T data) { this.data = data; }
        public T get()  { return data;}
     }

     Box<String> b = new Box<String>(); 
     b.set("Hello World!");
     String s= Box.get(); 

     Box<String> stringBox= new Box<String>();
     stringBox.set(new Integer(10)); //정수타입을 저장하려고 하면 컴파일 오류 발생 
     ``` 
* 제네릭 메소드 : 일반 클래스의 메소드에서도 타입 매개 변수 사용하여 제네릭 메소드 정의 가능 (타입 매개 변수의 범위가 메소드 내부로 제한됨)
    ```java
    public class MyArrayAlg{
        public static <T> T getLast(T[] a){ //제네릭 메소드 정의 
            return a[a.length -1]; 
        }
    }

    public class MyArrayAlgTest{ 
        public static void main(String[] args){
            String[] language = {"C++","C#","JAVA"};
            String last= MyArrayAlg.getLast(language); //last는 JAVA
            System.out.println(last); 
        }
    }
    ``` 
    * ex> 
        ```java
        public class GenericMethodTest{
            public static <T> void printArray(T[] array){
                for (T element: array){
                    System.out.print("%s",element); 
                }
                System.out.println(); 
            }

            public static void main(String[] args){
                Integer[] iArray={10,20,30,40,50}; 
                Double[] dArray={1.1,1.2,1.3,1.4,1.5}; 
                Character[] cArray={'K','O','R','E','A'}; 

                printArray(iArray); 

                printArray(dArray); 
                printArray(cArray); 
            }
        }
        ``` 
        ```
        10 20 30 40 50
        1.1 1.2 1.3 1.4 1.5
        K O R E A
        ``` 
## 컬렉션 
* `컬렉션`: 자바에서 `자료 구조`(리스트, 스택, 큐, 집합, 해쉬 테이블 등) 를 구현한 클래스 
* 역사
    * 초기: Vector, Stack, HashTable, Bitset, Enumeration 
    * 버전 1.2부터는 풍부한 컬렉션 라이브러리 제공 
        * 인터페이스와 구현을 분리 (ex. List 인터페이스를 ArrayList와 LinkedList가 구현)
* `컬렉션 인터페이스와 컬렉션 클래스로 나누어서 제공.` : 컬렉션 인터페이스를 구현한 클래스도 함께 제공하므로 이것을 간단하게 사용할 수도 있고 아니면 각자 필요에 맞추어 인터페이스를 자신의 클래스로 구현할 수도 있음. 
    * ex> Collection > Set/List > HashSet,TreeSet,SortedSet/ ArrayList,LinkedList 
    * ex> Map > hashMap, TreeMap, SortMap 
    * ex> Queue > ArrayDeque, LinkedList, PriorityQueue 
* 컬렉션 인터페이스 
    1. Collection: 모든 자료구조의 부모 interface. 객체의 모임을 나타냄
    2. Set: 집합(중복된 원소 가지지x)을 나타내는 자료구조
    3. List: 순서가 있는 자료구조. 중복된 원소 가질 수 o
    4. Map: 키와 값이 연관되어 있는 사전과 같은 자료구조
    5. Queue: 순서대로 나가는 자료구조 
* 컬렉션 특징 
    * 컬렉션은 `제네릭`을 사용
    * 컬렉션에는 int, double과 같은 기초 자료형을 쓰면 안됨 -> `클래스만 가능` 
        * `래퍼 클래스`인 Integer, Double 사용 가능 
    * 기본 자료형을 저장하면 자동으로 래퍼 클래스의 객체로 변환됨 (`오토박싱 auto boxing`)
* 컬렉션의 모든 요소 방문하기 
    ```java
    String a[] = new Sring[]{"A","B","C","D","E"}; 
    List<String> list= Arrays.asList(a); 
    ```
    1. for 구문 
    ```java
    for (int i=0;i<list.size();i++){
        System.out.println(list.get(i)); 
    }
    ```
    2. for-each 구문 사용 
    ```java
    for (String s:list){
        System.out.println(s); 
    }
    ```
    3. 반복자 (iterator) 사용 
        * 반복자: 특별한 타입의 객체. 컬렉션의 원소들을 접근하는 것이 목적. ArrayList 뿐만 아니라 모든 컬렉션에 적용 가능 
    ```java
    String s; 
    Iterator a= list.iterator(); 
    while(a.hasNext()){ //hasNext(): 아직 방문하지 않은 원소가 있으면 true
        s = (String)e.next(); //다음 원소 반환 
        System.out.println(s); 
    }
    ``` 
    4. Stream 라이브러리 이용
        * forEach 메소드와 람다식 사용 
    ```java
    list.forEach((n)->System.out.println(n)); 
    ``` 
* Java 8 버전 이후 함수 변화
    * 이전: 함수는 값(value)가 아니었음. 함수를 변수에 저장할 수 없었고, 매개변수로 전달할 수도 없었음. 
    * Java 8 이후: 함수가 값이 되면서 함수도 변수에 저장 가능, 함수를 매개변수로 받을 수 있음, 함수를 변환 가능 

## 람다식 (lambda expression)
* `람다식`: 나중에 실행될 목적으로 다른 곳에 전달될 수 있는 코드블록 
    * 0개 이상의 매개변수 가질 수 있음
    * 화살표 -> 는 람다식에서 매개 변수와 몸체 구분 
    * 매개 변수의 형식을 명시적으로 선언 가능, 문맥에서 추정될 수 있음 
        * (int a)는 a와 동일
        * 빈 괄호는 매개 변수가 없음을 나타냄.  
    * 단일 매개 변수이고 타입이 유추 가능한 경우에는 괄호 사용 필요 없음. ex> a -> return a*a 
    * 몸체에 하나 이상의 문장이 있으면 { }로 묶어야함. 
* 람다식의 활용: 자바에서 그래픽 사용자 인터페이스 코드를 작성할 때 함수 몸체를 전달하고 싶은 경우 
    ```java 
    //이전의 방법
    button.addActionListener(new ActionListener(){
        @Override
        public void actionPerformed(ActionEvent e){
            System.out.println("버튼 클릭!"); 
        }
    }) 
    ```
    ```java
    //람다식을 이용한 방법
    button.addActionListener( (e)-> {
        System.out.println("버튼 클릭!"); 
    })
    ``` 
    * 자바에서 스레드를 작성하려면 먼저 Runnable 인터페이스를 구현하는 클래스부터 작성해야함 
    ```java
    //이전의 방법
    new Thread(new Runnable(){
        @Override
        public void run(){
            System.out.println("스레드 실행"); 
        }
    }).start(); 
    ```
    ```java
    //람다식을 이용한 방법 
    new Thread( ()-> System.out.println("스레드 실행")).start(); 
    ```
    * 람다식을 사용하면 배열의 모든 요소를 출력하는 코드에서 forEach()와 같은 함수형 프로그래밍 사용 가능 
    ```java
    //이전의 방법
    List<Integer> list=Arrays.asList(1,2,3,4,5);
    for (Integer n:list){
        System.out.println(n); 
    }
    ```
    ```java
    //람다식을 이용한 방법
    List<Integer> list= Arrays.asList(1,2,3,4,5); 
    list.forEach(n-> System.out.println(n)); 
    ``` 
* 동작 매개 변수화(behavior parameterization): 고객의 빈번한 요구 사항 변경을 처리할 수 있는 소프트웨어 개발 패턴 - 사용자의 요구를 담은 코드 블록을 생성하고 이것을 프로그램의 다른 부분에 전달
    * 스트림 라이브러리를 사용하면 ArrayList와 같은 컬렉션에서 조건을 주어서 다양한 처리를 순차적으로 연결할 수 있음 

## 컬렉션의 예: Vector 클래스 
* `Vector`: java.util 패키지에 있는 컬렉션의 일종. 가변 크기의 배열 (dynamic array) 구현하고 있음 
```java
import java.util.*;

public class VectorExample1{
    public static void main(String[] args){
        Vector<String> vec=new Vector<String>(2); 
        vec.add("Apple");
        vec.add("Orange");
        vec.add("Mango");

        System.out.println("벡터의 크기:"+vec.size());
        Collections.sort(vec); 
        for(String s:vec){
            System.out.print(s+" "); 
        }
    }
}
```
```
벡터의 크기: 3
Apple Mango Orange
```

## 컬렉션의 예: ArrayList 
* `ArrayList`: 배열(Array)의 향상된 버전 또는 가변 크기의 배열 
    * 생성: `ArrayList<String> list= new ArrayList<String>(); ` 
* Vector vs ArrayList
    * Vector는 `스레드 간의 동기화`를 지원하는데 반하여 ArrayList는 동기화를 하지 않아서 Vector보다 성능은 우수 

## 컬렉션의 예: LinkedList 
* `LinkedList`: 빈번하게 삽입과 삭제가 일어나는 경우에 사용  
*  ArrayList vs LinkedList  
    * ArrayList는 인덱스를 가지고 원소에 접근할 경우, 항상 일정한 시간만 소요됨. ArrayList는 리스트의 각각의 원소를 위하여 노드 객체를 할당할 필요가 있음. 동시에 많은 원소를 이동하여야하는 경우는 System.arraycopy() 사용 가능 
    * 만약 리스트의 처음에 빈번하게 원소를 추가하거나 내부의 원소 삭제를 반복하는 경우에는 LinkedList를 사용하는 것이 나음. 이들 연산은 LinkedList에서는 일정한 시간만 걸리지만 ArrayList에서는 원소의 개수에 비례하는 시간이 소요됨.

## 컬렉션의 예: Set
* `Set`: 원소의 중복을 허용하지 않아요 
    * HashSet: HashSet은 해쉬 테이블에 원소를 저장하기 때문에 성능면에서 가장 우수. but 원소들의 순서가 일정하지 않음
    * TreeSet: 레드-블랙 트리에 원소를 저장. 값에 따라 순서가 결정됨. HashSet보다는 느림 
    * LinkedHashSet: 해쉬 테이블과 연결 리스트를 결합한 것. 원소들의 순서는 삽입되었던 순서와 같음 
        * s1.containsAll(s2): s2가 s1의 부분집합이면 참
        * s1.addAll(s2): s1을 s1과 s2의 합집합으로 만듦
        * s1.retainAll(s2): s1을 s1과 s2의 교집합으로 만듦 
        * s1.removeAll(s2): s1을 s1과 s2의 차집합으로 만듦 
    ```java
    import java.util.*; 

    public class FindDupplication{

        public static void main(String[] args){
            Set<String> s= new HashSet<String>(); 
            String[] sample={"사과","사과","바나나","토마토"}; 
            for (String s: sample)
                if (!s.add(a))
                    System.out.println("중복된 단어: "+a); 
            System.out.println(s.size()+"중복되지 않은 단어:"+s); 
        }
    }
    ```
    ```
    중복된 단어: 사과
    3 중복되지 않은 단어: [토마토, 사과, 바나나]
    ```

## Map 
* 많은 데이터 중 원하는 데이터를 빠르게 찾을 수 있는 자료구조 
* 사전과 같은 자료구조
* Map의 모든 요소 방문하기
    1. for-each 구문과 keySet() 사용 
    ```java
    for (String key: map.keySet()){
        System.out.println("key="+key+", value="+map.get(key)); 
    }
    ```
    Java10 이상의 버전부터는 변수 타입 추론 사용 가능 
    ```java
    for (var key: map.keySet()){
        System.out.println("key="+key+", value="+map.get(key)); 
    }
    ``` 
    2. 반복자 사용 
    ```java
    Iterator<String> it= map.keySet().iterator(); 
    while(it.hasNext()){
        String key= it.next(); 
        System.out.println("key="+key+", value="+map.get(key)); 
    }
    ``` 
    3. Stream 라이브러리 사용 
    ```java
    map.forEach( (key,value)->{
        System.out.println("key="+key+", value="+value); 
    }); 
    ```

## 큐(queue)
* 큐는 후단(tail)에서 원소를 추가하고 전단(head)에서 원소를 삭제  
* 자바에서 큐는 인터페이스로 정의됨. 이 인터페이스를 구현한 3개의 클래스가 주어짐 -> ArrayDeque, LinkedList, PriorityQueue 
* 우선순위큐: 원소들이 무작위로 삽입되었어도 정렬된 상태로 원소들을 추출함. 즉 remove()를 호출할 때마다 가장 작은 원소가 추출됨 

## Collections 클래스 
* 정렬, 섞기, 탐색 -> 여러 유용한 알고리즘을 구현한 메소드 제공 
* `정렬` : 데이터를 어떤 기준에 의하여 순서대로 나열하는 것  
* `섞기` : 리스트에 존재하는 정렬을 파괴하여 원소들의 순서를 랜덤하게 만듦 
* `탐색` : 리스트 안에서 원하는 원소를 찾는 것. 
    * 리스트가 정렬되어 있지 않은 상태라면.. -> 처음부터 모든 원소 방문해야함 (선형 탐색)
    * 리스트가 정렬된 상태라면.. -> 중간에 있는 원소와 먼저 비교 (이진 탐색 Collections.binarySearch(list,key))