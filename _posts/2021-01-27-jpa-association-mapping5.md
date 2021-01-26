---
title: JPA) 연관관계 매핑3. 다양한 연관관계 매핑으로 코드 수정 
author: cotchan 
date: 2021-01-27 04:30:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 배송, 카테고리 엔티티 추가

+ 주문과 배송의 연관관계는 1:1(`@OneToOne`)
+ 상품과 카테고리의 연관관계는 N:M(`@ManyToMany`)

---

+ `Delivery Entity`

```java
@Entity
public class Delivery {

    @Id
    @GeneratedValue
    private Long id;

    private String city;
    private String street;
    private String zipcode;
    private DeliveryStatus status;

    @OneToOne(mappedBy = "delivery")
    private Order order;
}
```

---

+ `Category Entity`

```java
@Entity
public class Category {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    /**
     * 자식입장에서는 부모가 하나이므로
     */
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Category parent;

    /**
     * 여러 개의 자식을 두는 경우
     */
    @OneToMany(mappedBy = "parent")
    private List<Category> child = new ArrayList<>();
}
```

---

## 2. 연관관계 매핑

---

## 2-1. Delivery와 Order 1:1 매핑

+ `Delivery Entity`

```java
@Entity
public class Delivery {

    @Id
    @GeneratedValue
    private Long id;

    private String city;
    private String street;
    private String zipcode;
    private DeliveryStatus status;

    @OneToOne(mappedBy = "delivery")
    private Order order;
}
```

---

+ `Order Entity`

```java
@Entity
@Table(name = "ORDERS")
public class Order {

    @Id
    @GeneratedValue
    @Column(name = "ORDER_ID")
    private Long id;

    /**
     * Order와 Delivery의 1:1 매핑 정보 추가
     */
    @OneToOne
    @JoinColumn(name = "DELIVERY_ID")
    private Delivery delivery;
}
```

---

## 2-2. Category와 ITEM N:M 매핑

+ `Category Entity`

```java
@Entity
public class Category {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    /**
     * 자식입장에서는 부모가 하나이므로
     */
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Category parent;

    /**
     * 여러 개의 자식을 두는 경우
     */
    @OneToMany(mappedBy = "parent")
    private List<Category> child = new ArrayList<>();

    @ManyToMany
    @JoinTable(name = "CATEGORY_ITEM",
            joinColumns = @JoinColumn(name = "CATEGORY_ID"),
            inverseJoinColumns = @JoinColumn(name = "ITEM_ID")
    )
    private List<Item> items = new ArrayList<>();
}
```

---

+ `Item Entity`

```java
@Entity
public class Item {

    @Id @GeneratedValue
    @Column(name = "ITEM_ID")
    private Long id;

    private String name;
    private int price;
    private int stockQuantity;

    @ManyToMany(mappedBy = "items")
    private List<Category> categories = new ArrayList<>();
}
```

+ 그러면 테이블이 아래와 같이 생성되는 걸 볼 수 있습니다.

```java
//SQL
create table CATEGORY_ITEM (
   CATEGORY_ID bigint not null,
    ITEM_ID bigint not null
)
```

---

## 3. 기타 참고사항

+ **테이블의 N:M 관계는 `중간 테이블을 이용`해서 1:N, N:1로 매핑해줍니다.**
+ 실전에서는 @ManyToMany 사용 X

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping5-01.png)

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
