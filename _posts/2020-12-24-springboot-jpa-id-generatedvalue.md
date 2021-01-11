---
title: Spring-Boot) @Id, @GeneratedValue, @Column (PK 매핑하는 방법)
author: cotchan 
date: 2020-12-24 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_JPA]
tags: [spring-boot] 
---

+ 이 글은 아래 포스팅을 참고하여 작성한 글입니다. 
+ 계속 업데이트할 예정입니다.

---


## 1. JPA 엔티티 매핑

+ `@Entity`
+ `javax.persistence.Entity`
+ **@Entity 어노테이션을 달면 이제부터 이 Entity는 JPA가 관리한다는 의미입니다.**

```java
//Member.java
@Entity
public class Member {
    //...
}
```

---

## 2. @Id, @GeneratedValue

+ 기본키를 할당하는 방법으로는 `두 가지`가 있습니다.
  + **`직접할당`**
    + 기본 키를 어플리케이션에서 직접 할당 해주는 방법
  + **`자동생성`**
    + 데이터베이스가 자동으로 할당해주는 방법(예를 들어, 오라클은 sequence, Mysql의 auto_increment)

+ 기본 키를 직접 할당하기 위해서는 `@Id` 어노테이션만 사용하면 됩니다.
+ 자동 생성 전략을 사용하기 위해서는 `@Id`에 `@GeneratedValue`를 추가하고 원하는 키 생성 전략을 선택하면 됩니다.

---

## 2-1. @Id 

1. **`@Id`는 해당 프로퍼티가 테이블의 PK역할을 한다는 것을 의미합니다.**
  +  **즉 `@Id`를 사용하여 PK를 매핑해줍니다.**

---

## 2-2. @GeneratedValue
  
+ **`@GeneratedValue`는 PK 값을 위한 자동 생성 전략을 명시합니다.** 
+ DB에 값을 넣으면 DB가 알아서 ID를 자동으로 생성해주는 것을 `IDENTITY` 전략이라고 합니다.


+ 기본키 자동생성 방법
  + `IDENTITY`
    + 기본 키 생성을 데이터베이스에 위임하는 방법(DB에 의존적)
      +  주로 Mysql, PostgresSQL, SQL Server, DB2에서 사용합니다.  
  + `SEQUENCE`
    + 데이터베이스 시퀀스를 사용해서 기본 키를 할당하는 방법(DB에 의존적)
      + 주로 시퀀스를 지원하는 Oracle, PostgresSQL, DB2, H2에서 사용합니다.
  + `TABLE`
    + 키 생성 테이블을 사용하는 방법
      + 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만드는 방법입니다. 
      + 테이블을 사용하므로, 데이터베이스 벤더에 상관없이 모든 데이터베이스에 적용이 가능합니다.
  + `AUTO`
    + 데이터베이스 벤더에 의존하지 않고, 데이터베이스는 기본키를 할당하는 방법
      + 데이터베이스에 따라서 IDENTITY, SEQUENCE, TABLE 방법 중 하나를 자동으로 선택해주는 방법입니다.
      + 예를 들어, Oracle일 경우 SEQUeNCE를 자동으로 선택해서 사용합니다. 따라서 데이터베이스를 변경해도 코드를 수정할 필요가 없습니다.

---

## 3. Sample Code

```java
//Member.java
@Entity
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

---

## 4. @Column

+ **필드와 매핑할 테이블의 컬럼 이름을 정할 수 있습니다.**
+ 일반적으로 PK 지정 시 `{테이블명_id}`로 많이 사용합니다.


```java
//Member Entity
@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    private String name;
}
```

---

```java
//Order Entity
@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {

    @Id @GeneratedValue
    @Column(name = "order_id")
    private Long id;
}
```

---

+ 출처
  + [@Id와 @GeneratedValue 애노테이션](https://jsaver.tistory.com/entry/Id%EC%99%80-GeneratedValue-%EC%95%A0%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98)
  + [Spring Data JPA 기본키 매핑하는 방법](https://ithub.tistory.com/24)
  + [[JPA] 필드와 컬럼 매핑](https://ict-nroo.tistory.com/113)
