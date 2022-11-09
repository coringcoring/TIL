* CoreApplication.java
    ```java
    package hello.core;

    import org.springframework.boot.SpringApplication;
     org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class CoreApplication {

        public static void main(String[] args) {
            SpringApplication.run(CoreApplication.class, args);
        }

    }
    ```

* 회원 도메인 설계
1. 회원을 가입하고 조회 가능
2. 회원은 일반/vip 등급 있음
3. 회원 데이터는 자체 db를 구축 가능, 외부 시스템과 연동 가능
    * 회원 서비스: MemberServiceImpl 
        ![구조](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20111911.png)
```java
package hello.core.member;

public class Member {
    private Long id;
    private String name;
    private Grade grade;

    public Member(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }
}
```

```java
package hello.core.member;

public interface MemberRepository {

    void save(Member member);

    Member findById(Long memberId);
}
```

```java
package hello.core.member;

import java.util.HashMap;
import java.util.Map;

public class MemoryMemberRepository implements  MemberRepository{

    private static Map<Long,Member> store = new HashMap<>();
    //해시맵 쓰면 동시성의 이슈가 발생하지만 예시기 때문에 해시맵 사용함

    @Override
    public void save(Member member){
        store.put(member.getId(),member);
    }
    @Override
    public Member findById(Long memberId){
        return store.get(memberId);
    }
}
```

```java
package hello.core.member;

public interface MemberService {
    void join(Member member);

    Member findMember(Long memberId);
}
```

```java
package hello.core.member;

public enum Grade {
    BASIC,
    VIP
}

```

```java
package hello.core.member;

public class MemberServiceImpl implements MemberService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```
* 테스트 결과
    ![테스트](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20112801.png)
* 테스트 코드 작성 결과 
    ![테스트2](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20113249.png)
* 설계한 도메인의 문제점
    * 의존성의 문제점 발생
    

---
* 주문과 할인 도메인 설계 
1. 주문생성: 클라이언트는 주문 서비스에 주문 생성을 요청
2. 회원 조회: 할인을 위해서는 회원 등급이 필요. 그래서 주문 서비스는 회원 저장소에서 회원을 조회
3. 할인 적용: 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임
4. 주문 결과 반환: 주문 서비스는 할인 결과를 포함한 주문 결과를 반환함 

```java
package hello.core.order;

public class Order {

    private Long memberId;
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        this.memberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

    public int calculatePrice(){
        return itemPrice-discountPrice;
    }

    public Long getMemberId() {
        return memberId;
    }

    public void setMemberId(Long memberId) {
        this.memberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "memberId=" + memberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }
}
```

```java
package hello.core.order;

public interface OrderService {
    Order createOrder(Long memberId,String itemName, int itemPrice);
}
```

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;

public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository=new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy= new FixDiscountPolicy();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member=memberRepository.findById(memberId);
        int discountPrice=discountPolicy.discount(member,itemPrice);

        return new Order(memberId,itemName,itemPrice,discountPrice) ;
    }
}
```

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    //return 할인 대상 금액
    int discount(Member member,int price);
}
```

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{

    private int discountFixAmount=1000; //1000원 할인

    @Override
    public int discount(Member member,int price){
        if (member.getGrade() == Grade.VIP){
            return discountFixAmount;
        }
        else{
            return 0;
        }
    }
}
```

```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.order.Order;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class OrderApp {

    public static void main(String[] args) {
        MemberService memberService=new MemberServiceImpl();
        OrderService orderService=new OrderServiceImpl();

        Long memberId =1L;
        Member member= new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order=orderService.createOrder(memberId,"itemA",10000);

        System.out.println("order= "+order);
        System.out.println("order= "+order.calculatePrice());
    }
}
```
* 테스트 결과
![테스트코드 OrderApp](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20170942.png)

![테스트코드 OrderApp2](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20171030.png)

![테스트 코드 작성 후 결과](./imgs/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-11-09%20171400.png)