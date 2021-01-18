---
title: JPA) 엔티티 매핑1. 객체와 테이블 매핑
author: cotchan 
date: 2021-01-19 05:35:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 엔티티 매핑

1. **객체와 테이블 매핑**
  + `@Entity`, `@Table`
2. **필드와 컬럼을 매핑**
  + `@Column`
3. **기본 키 매핑**
  + `@Id`
4. **연관관계 매핑**
  + `@ManyToOne`, `@JoinColumn`

---

## 2. @Entity

```java
@Entity
public class Member {
  //...
}
```

+ **@Entity가 붙은 클래스는 JPA가 관리합니다.** 

+ JPA를 사용해서 테이블과 매핑할 클래스는 `@Entity`가 필수입니다.

+ **주의**
  + **기본 생성자는 필수입니다**
    + 파라미터가 없는 public 또는 protected 생성자
  + final 클래스, enum, interface, inner 클래스는 안됩니다.
  + 내가 DB에 저장하고 싶은 필드에는 final 사용 X

---

+ **@Entity 속성**(크게 중요한 건 X)

+ 그냥 Name이라는 속성을 지정할 수 있습니다. 
+ @Entity의 `기본값`은 **현재 클래스 이름과 동일한 이름을 사용**합니다.
+ @Entity Name 속성을 사용해야 하는 경우는 예를 들어, 다른 패키지에 같은 이름의 클래스가 있는 경우 구분을 하기 위해 사용합니다. 
+ 그냥 @Entity의 기본값을 쓰는 게 바람직합니다.

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-entity-maaping-02.png)

---

## 3. @Table

+ **매핑을 할 때 `TABLE명을 바꾸고 싶은 경우`에 사용합니다.**
+ 아래와 같이 @Table 속성을 주면 `'MBR'`이라는 테이블과 매핑을 합니다.

```java
@Entity
//@Table(name = "MBR")   //DB 테이블에 MBR이라고 되어있으면, 쿼리가 MBR라는 테이블에 insert하라고 나갑니다.
public class Member {
  //...
}
```

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-entity-maaping-01.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
