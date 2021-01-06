---
title: HelloShop) 엔티티 클래스 개발방법
author: cotchan
date: 2021-01-06 21:00:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---


## 1. 엔티티에는 가급적 Setter 사용 금지

+ 엔티티의 데이터는 조회할 일이 많으므로 `Getter`의 경우 모두 열어두는 것이 편리합니다.
    + Getter는 아무리 호출해도 호출하는 것만으로 어떤 일이 발생하지는 않습니다.
+ **하지만 `Setter`는 호출하면 데이터가 변합니다.**
    + Setter를 막 열어두면 가까운 미래에 엔티티가 어디에서 변경되는지 추적하기가 어려워집니다.
    + 그러므로 엔티티를 변경할 때는 Setter 대신에 변경 지점이 명확하도록 변경을 위한 `비즈니스 메서드를 별도로 제공`해야 합니다.


---

## 2. 모든 연관관계는 지연로딩으로 설정

+ **정말 중요**
+ 즉시 로딩(`EAGER`)는 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어렵습니다.
    + 특히 JPQL을 실행할 때 `N+1 문제`가 자주 발생합니다.
+ **실무에서 모든 연관관계는 지연로딩(`LAZY`)으로 설정해야 합니다.**
+ **연관된 엔티티를 함께 DB에서 조회해야 하면, `fetch join` 또는 `엔티티 그래프` 기능을 사용합니다.**
+ **`@XToOne`(OneToOne, ManyToOne) 관계는 default가 즉시로딩이므로 `직접 지연로딩`으로 `설정해야` 합니다.**


---

## 3. 컬렉션은 필드에서 초기화하기

+ **그리고 나서 손대지 말기!**

```java
@Entity
@Getter @Setter
public class Member {

    //...

    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

+ 컬렉션은 필드에서 바로 초기화하는 것이 안전합니다.
+ **`null` 문제에서 안전해집니다.**
+ 하이버네이트는 엔티티를 영속화할 때, 컬렉션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경합니다.
+ 만약 `getOrders()`처럼 임의의 메서드에서 컬렉션을 잘못 생성하면 하이버네이트 내부 메커니즘에 문제가 발생할 수 있습니다. 따라서 필드레벨에서 생성하는 것이 가장 안전하고, 코드도 간결합니다.
    + **그러므로 객체 생성 후 컬렉션 자체를 절대로 바꾸면 안됩니다. 그냥 있는 것을 그대로 써야합니다.**

```java
Member member = new Member(); 
System.out.println(member.getOrders().getClass()); 
em.persist(team); 
System.out.println(member.getOrders().getClass());

//출력 결과
class java.util.ArrayList
class org.hibernate.collection.internal.PersistentBag
``` 

---

## 4. 테이블, 컬럼명 생성 전략

+ 스프링부트를 사용하면 `SpringPhysicalNamingStrategy` 전략에 따라 네이밍을 바꿉니다.

1. 카멜 케이스 -> 언더스코어 (memberPoint -> member_point)
2. .(점) -> _(언더스코어)
3. 대문자 -> 소문자

---

## 5. Cascade 옵션

+ Cascade 옵션을 사용하면 연관관계가 있는 엔티티끼리 `persist나 삭제`가 연쇄적으로 일어나게 해줍니다.
+ **원래는 `Entity 당` 각 각 `persist를 호출`해야합니다.**

```java
@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {

    //...

    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

    /**
     * 원래는 Entity 당 각 각 Persist 호출해야 합니다.
     * persist(orderItemA)
     * persist(orderItemB)
     * persist(orderItemC)
     *
     * cascade를 셋팅하면
     * persist(order)만 해주면 위 3줄 코드는 지워도 됩니다.
     */

    //Order를 저장할 때 delivery의 값만 셋팅해놓으면 order를 저장할 때 Delivery도 persist 해줍니다.
    //원래는 각 각을 persist 해줘야함.
    //기본은 모든 Entity는 저장하고 싶으면 각 각 persist 해야합니다.
    //여기에 값만 해놓고 Order를 persist하면 얘도 persist 됩니다.
    @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    @JoinColumn(name = "delivery_id")
    private Delivery delivery;
}
```


---

## 6. 연관관계 편의 메서드

+ `양방향 연관 관계` `셋팅`하려면
    + DB에 저장하는 건 연관관계 주인만 해도 되지만, 값을 셋팅하는 건 양쪽에 다 해주는 게 좋습니다.
+ **양방향 연관 관계에 있는 객체중 하나를 set할 때 `연관관계가 있는 두 개`를 `원자적으로 묶는 메서드`**
    + 아래 예시 코드의 경우 `setter`에 만들어줬습니다.  
+ 연관관계 편의 메서드의 위치는 양쪽에서 `컨트롤하는 쪽`이 가지고 있는 게 낫습니다.

```java
//Order Entity
@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {

    //...

    //==연관관계 편의 메서드 ==//
    //Member를 set할 때 연관관계가 있는 두 개를 원자적으로 묶는 메서드
    public void setMember(Member member) {
        this.member = member;
        member.getOrders().add(this);
    }

    //==연관관계 편의 메서드 ==//
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }

    //==연관관계 편의 메서드 ==//
    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
        delivery.setOrder(this);
    }
}
```
  

---

## 7. PK는 테이블명_id로

+ 객체는 `Member.id`라고 하면 Member의 타입이 있기 때문에 어디 소속인지가 명확합니다.
+ 반면에 테이블은 단순하게 `id`라고 하면 찾기가 쉽지 않습니다. 
+ 또한 join할 때도 불편합니다.
+ Table은 타입이 없으므로 관례상 위와 같은 `테이블명_id` 형태로 많이 사용합니다.

---

## 8. @ManyToMany 사용 금지

+ @ManyToMany를 사용하면 중간 테이블의 값을 수정할 수 없습니다. 그러므로 실무에서는 사용을 지양해야 합니다.

---

## 9. Sample Code

## 9-1. Order Entity

```java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {

    @Id @GeneratedValue
    @Column(name = "order_id")
    private Long id;

    //Order와 Member는 다대일 관계
    @ManyToOne(fetch = FetchType.LAZY)
    //매핑을 뭘로 할거냐를 적어주면 됩니다.
    //FK의 이름이 member_id가 됩니다.
    //연관관계의 주인이므로 얘 값을 셋팅하면 FK값이 다른 멤버로 변경이 됩니다.
    @JoinColumn(name = "member_id")
    private Member member;

    //order에 의해서 매핑이 된다는 뜻(나는 그냥 readonly 입니다)
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

    /**
     * 원래는 Entity 당 각 각 Persist 호출해야 합니다.
     * persist(orderItemA)
     * persist(orderItemB)
     * persist(orderItemC)
     *
     * cascade를 셋팅하면
     * persist(order)만 해주면 위 3줄 코드는 지워도 됩니다.
     */

    //Order를 저장할 때 delivery의 값만 셋팅해놓으면 order를 저장할 때 Delivery도 persist 해줍니다.
    //원래는 각 각을 persist 해줘야함.
    //기본은 모든 Entity는 저장하고 싶으면 각 각 persist 해야합니다.
    //여기에 값만 해놓고 Order를 persist하면 얘도 persist 됩니다.
    @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    @JoinColumn(name = "delivery_id")
    private Delivery delivery;

    //Hibernate가 자동으로 지원해주는 type
    private LocalDateTime orderDate;

    @Enumerated(EnumType.STRING)
    private OrderStatus orderStatus; //주문 상태 [ORDER, CANCEL]

    //==연관관계 편의 메서드 ==//
    //Member를 set할 때 연관관계가 있는 두 개를 원자적으로 묶는 메서드
    public void setMember(Member member) {
        this.member = member;
        member.getOrders().add(this);
    }

    //==연관관계 편의 메서드 ==//
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }

    //==연관관계 편의 메서드 ==//
    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
        delivery.setOrder(this);
    }
}
```

---

## 9-2. OrderItem Entity

```java
package jpabook.jpashop.domain;

import jpabook.jpashop.domain.item.Item;
import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;

@Entity
@Getter @Setter
public class OrderItem {

    @Id
    @GeneratedValue
    @Column(name = "order_item_id")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "item_id")
    private Item item;

    //하나의 Order가 여러 개의 OrderItem을 가질 수 있습니다.
    //OrderItem은 하나의 Order만 가질 수 있습니다.
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id")
    private Order order;

    private int orderPrice; //주문 가격
    private int count;  //주문 수량
}
```

---

```java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    private String name;

    @Embedded
    private Address address;

    //Member 입장에서는 Member와 Order는 일대다 관계
    //"나는 연관관계의 주인이 아닙니다"를 표시 해줌(mappedBy)
    //"나는 Order 테이블에 있는 member 필드에 의해서 매핑된 겁니다."라고 하는 것. (즉, 읽기 전용 이라는 것)
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

---

## 9-4. Delivery Entity

```java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;

@Entity
@Getter @Setter
public class Delivery {

    @Id @GeneratedValue
    @Column(name = "delivery_id")
    private Long id;

    @OneToOne(mappedBy = "delivery", fetch = FetchType.LAZY)
    private Order order;

    @Embedded
    private Address address;

    //Enum type은 이 어노테이션을 넣고
    //Ordinary랑 String을 넣을 수 있다. (Ordinary는 숫자로 들어감)
    //단 중간에 값이 추가 되면 망하므로 꼭 Ordinary말고 STRING을 써야한다.
    @Enumerated(EnumType.STRING)
    private DeliveryStatus status; //READY, COMP(배송준비, 배송)
}
```

---

## 9-5. Category Entity

```java
package jpabook.jpashop.domain;

import jpabook.jpashop.domain.item.Item;
import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter
public class Category {

    @Id @GeneratedValue
    @Column(name = "category_id")
    private Long id;

    private String name;

    //다대다 관계입니다.
    @ManyToMany
    //중간 테이블에 매핑을 해줍니다.
    //객체는 Collection 관계로 양쪽에 다대다를 해줘도 되는데
    //관계형 DB는 객체같이 불가능하기 때문에 일대다, 다대일로 풀어내는 중간 테이블이 필요합니다.
    @JoinTable(name = "category_item",
            joinColumns = @JoinColumn(name = "category_id"), //중간테이블에 있는 cateogry_id
            inverseJoinColumns = @JoinColumn(name = "item_id")) //category_item 테이블의 Item쪽으로 들어가는 번호
    private List<Item> items = new ArrayList<>();

    //계층 구조
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "parent_id")
    private Category parent;

    @OneToMany(mappedBy = "parent")
    private List<Category> child = new ArrayList<>();

    //==연관관계 편의 메서드 ==//
    public void addChildCategory(Category child) {
        this.child.add(child);
        child.setParent(this);
    }
}
```

---

## 9-6. Address Entity

```java
package jpabook.jpashop.domain;

import lombok.Getter;

import javax.persistence.Embeddable;

//어딘가에 내장이 될 수 있다는 의미
@Embeddable
@Getter
public class Address {

    private String city;
    private String street;
    private String zipcode;

    //JPA 구현 라이브러리가 객체를 생성할 때 리플렉션, 프록시 같은 기술을 써야하므로 기본 생성자가 필요합니다.
    //protected까지는 허용합니다.
    //이 기본 생성자는 JPA 스펙상 만든 것
    protected Address() {
    }

    //값 타입은 Immuatable하게 설계해야 합니다.
    //그러므로 생성할 때만 값이 생성이 되게 하는 것이 바람직합니다. (Setter 제공 X)
    public Address(String city, String street, String zipcode) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
    }
}
```










---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


