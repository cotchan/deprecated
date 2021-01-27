---
title: JPA) 연관관계 매핑4. 상속관계 매핑으로 코드 수정 
author: cotchan 
date: 2021-01-28 00:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 요구사항 추가

1. 상품의 종류는 음반, 도서, 영화가 있고 이후에 더 확장될 수 있습니다.
2. 모든 데이터는 등록일과 수정일이 필수입니다.

---

## 2. 도메인 모델

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapping-extend-example-01.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapping-extend-example-02.png)

---

## 3. 테이블

+ 싱글 테이블로 설계

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapping-extend-example-03.png)

---

## 4. 코드 예시 - 싱글 테이블 전략

+ **Item Entity**

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE) //조인 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item extends BaseEntity {

    @Id @GeneratedValue
    @Column(name = "ITEM_ID")
    private Long id;

    private String name;
    private int price;
    private int stockQuantity;
}
```

---

+ **Album**

```java
@Entity
public class Album extends Item {

    private String artist;
    private String etc;
}
```

---

+ **Book**

```java
@Entity
public class Book extends Item {

    private String author;
    private String isbn;
}
```

---

+ **Movie**

```java
@Entity
public class Movie extends Item {

    private String director;
    private String actor;
}
```

---

+ 데이터가 ITEM TABLE에 들어가 있는 것을 확인할 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapping-extend-example-04.png)

---

## 4-1. JOIN 전략으로 수정하는 경우

+ 아래 한 줄만 수정해주면 됩니다.
  + InheritanceType.SINGLE_TABLE => `InheritanceType.JOINED`

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED) //조인 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item extends BaseEntity {
}
```

---

+ 쿼리가 두 번 나가는 것을 확인할 수 있습니다.

```java
Hibernate: 
    /* insert jpabook.jpashop.domain.Book
        */ insert 
        into
            Item
            (name, price, stockQuantity, DTYPE, ITEM_ID) 
        values
            (?, ?, ?, 'Book', ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Book
        */ insert 
        into
            Book
            (author, isbn, ITEM_ID) 
        values
            (?, ?, ?)
```

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapping-extend-example-05.png)

---

## 5. Base Entity 추가

```java
@MappedSuperclass
public abstract class BaseEntity {

    private String createdBy;
    private LocalDateTime createdDate;
    private String lastModifiedBy;
    private LocalDateTime lastModifiedDate;
    //...
}
```

---

+ Item Entity에서 상속

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED) //조인 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item extends BaseEntity {
}
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
