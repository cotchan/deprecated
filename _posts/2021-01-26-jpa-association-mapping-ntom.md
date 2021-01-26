---
title: JPA) 연관관계 매핑3. 다대다[N:M] 
author: cotchan 
date: 2021-01-26 21:17:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 다대다 매핑 - 테이블

+ **다대다 매핑은 실무에서 사용하면 안됩니다.**

+ 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없습니다.
+ 연결 테이블을 추가해서 **일대다, 다대일 관계로 풀어내야 합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-ntom_01.png)

---

## 2. 다대다 매핑 - 객체

+ **객체는 컬렉션을 사용해서 객체 2개로 다대다 관계를 나타낼 수 있습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-ntom_02.png)

---

+ @ManyToMany 사용
+ @JoinTable로 연결 테이블을 지정합니다.
+ 다대다 매핑: 단방향, 양방향 모두 가능합니다.

---

## 3. 다대다 매핑의 한계

+ 편리해보이지만 실무에서 사용하지 않습니다.
+ 연결 테이블이 단순히 연결만 하고 끝나지 않습니다.
+ 주문시간, 수량 같은 데이터가 들어올 수 있습니다.
  + **그러나 중간테이블에는 오직 매핑 정보만 들어갈 수 있고, 추가정보를 넣을 수 없습니다.** 

---

## 4. 다대다 한계 극복 방법

+ **연결 테이블용 엔티티 추가 (연결 테이블을 엔티티로 승격)**
+ `@ManyToMany` -> `@OneToMany`, `@ManyToOne`

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-ntom_03.png)

---

## 4-1. 코드 예시

+ MemberProduct.java

```java
@Entity
public class MemberProduct {

    @Id
    @GeneratedValue
    private Long id;

    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;

    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;

    //필요한 필드를 추가하는 것도 가능합니다.
    private int count;
    private int price;
    private LocalDateTime orderDate;
}
```

---

+ Member.java

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @OneToMany(mappedBy = "member")
    private List<MemberProduct> memberProducts = new ArrayList<>();
}
```

---

+ Product.java

```java
@Entity
public class Product {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "product")
    private List<MemberProduct> memberProducts = new ArrayList<>();
}
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
