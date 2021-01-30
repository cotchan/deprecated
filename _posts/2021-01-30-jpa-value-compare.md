---
title: JPA) 값 타입의 비교
author: cotchan 
date: 2021-01-30 19:25:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 값 타입의 비교

+ 값 타입은 인스턴스가 달라도 그 안의 값이 같으면 같은 것으로 봐야 합니다.

+ 값 타입의 비교는 `동일성 비교`와 `동등성 비교`를 구분해서 사용해야 합니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-compare-01.png)

---

+ 그래서 값 타입은 equals() 메소드를 재정의해서 값 타입의 비교를 해야합니다. 
  + **equals() 메소드의 기본 형태는 == 비교 입니다.**
+ 자바의 primitive 타입은 그냥 == 비교를 해도 됩니다.
+ **특히 Embedded 타입 같은 경우는 equals()를 만들어줘서 동등성 비교를 해야 합니다.**

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
