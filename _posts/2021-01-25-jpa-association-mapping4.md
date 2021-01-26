---
title: JPA) 연관관계 매핑2. 양방향 연관관계로 코드 수정 
author: cotchan 
date: 2021-01-25 22:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 연관관계 매핑 수정

+ **먼저 가장 중요한 건, `단방향 매핑`으로 모든 관계를 끝내는 것입니다.**
  + 단방향 매핑이 더 바람직합니다.

+ 설계할 때는 단방향으로 모든 관계를 마무리 짓는 것이 바람직합니다.

---

+ BEFORE
  + 이전에는 엔티티에서 MEMBER_ID `FK 값을 매핑`해서 그대로 사용
 
+ AFTER
  + 이제는 `객체의 래퍼런스`가 MEMBER_ID랑 매핑이 되는 것입니다.


---

## 2. ORDER와 ORDER_ITEM

+ ORDER와 ORDER_ITEM의 관계는 1:다 관계입니다.

+ **당연히 테이블 설계상 '다'쪽에 외래키가 존재하게 됩니다.**

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping4-01.png)

---

## 3. ITEM과 ORDER_ITEM

+ ITEM과 ORDER_ITEM의 관계 역시 1:다 관계입니다.

+ **당연히 테이블 설계상 '다'쪽에 외래키가 존재하게 됩니다.**

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping4-01.png)

---

## 4. 연관 관계 설계 변경

+ **중요한 변동사항은 ORDER_ITEM이 FK값을 그대로 가지고 있는 게 아니라 객체를 가지고 있는 형태로 바꾼 것입니다.**

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping4-02.png)

---

+ Order 수정

```java
@Entity
@Table(name = "ORDERS")
public class Order {

    /**
     * Order 입장에서 Member는 ManyToOne
     * Member입장에서는 하나의 회원이 여러개의 주문을 날리니까 일대다
     * 반대로 Order 입장에서 나를 주문하는 회원은 1명이므로는 다대일
     */
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;

//수정한 부분
    /**
     * Order와 OrderItem 사이의 양방향 관계를 설정해주기 위함
     *
     * 얘랑 매핑이 되는 것입니다.
     *
     * //OrderItem.java
     * @ManyToOne
     * @JoinColumn(name = "ORDER_ID")
     * private Order order;
     */
    @OneToMany(mappedBy = "order")
    private List<OrderItem> orderItems = new ArrayList<>();
}
```

---

+ **`OrderItem` 수정**

```java
@Entity
public class OrderItem {

    @Id
    @GeneratedValue
    @Column(name = "ORDER_ITEM_ID")
    private Long id;

//    @Column(name = "ORDER_ID")
//    private Long orderId;

    @ManyToOne
    @JoinColumn(name = "ORDER_ID")
    private Order order;

//    @Column(name = "ITEM_ID")
//    private Long itemId;

    @ManyToOne
    @JoinColumn(name = "ITEM_ID")
    private Item item;
}
```

---

+ Member 수정 (Order와 양방향 연관관계로)

+ Member에서도 자신의 주문 리스트를 확인할 수 있도록 위 사진처럼 필드에 `orders: List`를 추가합니다.

```java
@Entity
public class Member {
    /**
     * @ManyToOne의 반대입니다.
     * Order의 입장에서 @ManyToOne이었으므로.
     * 또한, 관례로 컬렉션을 사용하는 경우 필드 주입방식으로 객체를 바로 생성해줍니다.
     *
     * 얘가 연관관계의 주인입니다.
     * //Order.java
     * @ManyToOne
     * @JoinColumn(name = "MEMBER_ID")
     * private Member member;
     */
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
