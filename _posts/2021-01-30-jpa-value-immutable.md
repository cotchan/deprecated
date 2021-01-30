---
title: JPA) 값 타입과 불변 객체
author: cotchan 
date: 2021-01-30 18:48:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 값 타입 공유 참조

+ 임베디드 타입 같은 값 타입을 공유할 수 있을 때 문제가 생길 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-immutable-01.png)

---

```java
Address address = new Address("city", "street", "10000");

Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(address);
em.persist(member);

Member member2 = new Member();
member2.setUsername("member2");
member2.setHomeAddress(address);
em.persist(member2);

//Update Query가 두 번 나가게 됩니다. => member, member2
member.getHomeAddress().setCity("newCity");
```

---

+ 위와 같은 Side Effect를 방지하기 위해서는 값을 복사해서 사용해야 합니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-immutable-02.png)

---

```java
Address address = new Address("city", "street", "10000");

Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(address);
em.persist(member);


Address copyAddress = new Address(address.getCity(), address.getStreet(), address.getZipCode());


Member member2 = new Member();
member2.setUsername("member2");
member2.setHomeAddress(address);
em.persist(member2);

//Update를 해도 각 각의 Address가 유지됩니다.
member.getHomeAddress().setCity("newCity");
```

---

## 2. 객체 타입의 한계

+ 항상 값을 복사해서 사용하면 공유 참조로 인해 발생하는 부작용을 피할 수 있습니다.
+ 문제는 임베디드 타입처럼 **직접 정의한 값 타입은 자바의 기본 타입이 아니라 객체 타입**입니다.
+ 자바 기본 타입에 값을 대입하면 값을 복사합니다.
+ **하지만 객체 타입은 참조 값을 직접 대입하는 것을 막을 방법이 없습니다.**
  + 누군가 실수로 참조 값을 직접 대입해도 Compile 레벨에서 잡아낼 수 있는 방법이 없습니다.
+ **`결론`은 객체의 공유 참조는 피할 수 없습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-immutable-03.png)

---

## 3. 불변 객체(Immutable Object)

+ 객체 타입을 수정할 수 없게 만들면 **부작용을 원천 차단할 수 있습니다.**
+ **그러므로 값 타입은 불변 객체(immutable object)로 설계해야 합니다.**
  + 불변 객체란 생성 시점 이외에는 절대 값을 변경할 수 없는 객체를 말합니다.
+ 생성자로만 값을 설정하고 수정자(Setter)를 만들지 않으면 됩니다.
  + 가장 간단한 방법

+ 만약 값을 수정하고 싶다면 **불변 객체를 통으로 다시 만들어서** 값을 셋팅해줘야 합니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
