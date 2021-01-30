---
title: JPA) 임베디드 타입(복합 값 타입)
author: cotchan 
date: 2021-01-30 17:37:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 임베디드 타입

+ 새로운 값 타입을 직접 정의할 수 있습니다.

+ JPA는 임베디드 타입(embedded type)이라 합니다.

+ 주로 기본 값 타입을 모아서 만들어서 복합 값 타입이라고도 합니다.

+ **(중요)int, String과 같은 값 타입**
  + 엔티티 타입이 아닙니다.

---

## 2. 임베디드 타입 사용 예시

+ workPeriod, homeAddress라는 타입을 만든 경우

+ Before(임베디드 타입 적용 전)

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-embedded-01.png)

---

+ After(임베디드 타입 적용 후)
  + 즉, 클래스 두 개를 새로 만들었습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-embedded-02.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-embedded-03.png)

---

## 3. JPA에서 임베디드 타입 사용방법

+ **@Embeddable**
  + 값 타입을 정의하는 곳에 표시

+ **@Embedded**
  + 값 타입을 사용하는 곳에 표시

+ 기본 생성자 필수

---

## 3-1. 임베디드 타입을 정의하는 곳

+ 값 타입을 정의하는 곳에서는 "이것이 값 타입이다"라고 JPA에게 알려줘야 합니다.

```java
@Embeddable
public class Period {
    
    private LocalDateTime startDate;
    private LocalDateTime endDate;
}
```

```java
@Embeddable
public class Address {
    
    private String city;
    private String street;
    private String zipcode;
}
```

---

## 3-2. 임베디드 타입을 사용하는 곳

+ 값 타입을 사용하는 곳에서는 @Embedded를 사용합니다.

```java
@Entity
public class Member {
    
    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    //기간
    @Embedded
    private Period workPeriod;

    //주소
    @Embedded
    private Address homeAddress;
}
```

---

## 4. 임베디드 타입의 장점

+ 재사용 가능성

+ 높은 응집도

+ Period.isWork()처럼 해당 값 타입만 사용하는 의미있는 메소드를 만들 수 있습니다.

+ 임베디드 타입을 포함한 모든 값 타입은, 값 타입을 소유한 엔티티의 생명주기에 의존합니다.
  + 값 타입이라는 건 Entity가 사라질 때 함께 사라집니다. :)

---

## 5. 임베디드 타입과 테이블 매핑

+ DB 테이블 입장에서는 바뀔 것이 없습니다. 반대쪽이 값 타입을 쓰든 안 쓰든 회원 테이블은 똑같습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-embedded-04.png)

---

+ 임베디드 타입은 엔티티의 값일 뿐입니다.

+ **임베디드 타입을 사용하기 전과 후에 `매핑하는 테이블은 같습니다.`**

+ 객체와 테이블을 아주 세밀하게 매핑하는 것이 가능합니다.

+ 잘 설계한 ORM 애플리케이션은 매핑한 테이블의 수보다 클래스의 수가 더 많습니다.

---

## 6. 임베디드 타입과 연관관계

+ PhoneNumber라는 임베디드 타입이 자신의 필드로 Entity를 들고 있을 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-embedded-05.png)

---

## 7. 속성 재정의(@AttributeOverride)

+ 한 엔티티에서 같은 값 타입을 사용하는 경우?
  + 컬럼 명이 중복되게 됩니다.

+ @AttributeOverrides, @AttributeOverride를 사용해서 컬럼명 속성을 재정의할 수 있습니다.

---

## 8. 임베디드 타입과 null

+ 임베디드 타입의 값이 null이면 매핑한 컬럼 값(임베디드 타입의 내부 필드 값)은 모두 null이 됩니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
