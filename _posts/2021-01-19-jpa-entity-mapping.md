---
title: JPA) 엔티티 매핑1. 객체와 테이블 매핑
author: cotchan 
date: 2021-01-14 04:55:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

![Desktop View](/assets/img/post/jpa/2020-12-10-spring-boot-how-to-build.png)

---

## 엔티티 매핑

1. **객체와 테이블 매핑**
  + `@Entity`, `@Table`
2. **필드와 컬럼을 매핑**
  + `@Column`
3. **기본 키 매핑**
  + `@Id`
4. **연관관계 매핑**
  + `@ManyToOne`, `@JoinColumn`

---

## @Entity

```java
@Entity
public class Member {
  //...
}
```

+ @Entity가 붙은 클래스는 JPA가 관리합니다. 

+ JPA를 사용해서 테이블과 매핑할 클래스는 `@Entity`가 필수입니다.

+ **주의**
  + **기본 생성자는 필수입니다**
    + 파라미터가 없는 public 또는 protected 생성자
  + final 클래스, enum, interface, inner 클래스는 안됩니다.
  + 내가 DB에 저장하고 싶은 필드에는 final 사용 X

---

@Entity 속성 

별로 중요한 건 X

그냥 Name이라는 속성을ㅈ ㅣ정할 수 있습니다. 기본값은 현재 클래스 이름고 ㅏ동일한 이름을 사용합니다.

예를 들어, 다른 패키지에 같은 이름의 클래스가 있는 경우, 하나는 다른 이름을

JPA가 내부적으로 구분하는 이름입니다. 그ㅑㄴㅇ 기본값을 써야합니다.


---

## @Table

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
