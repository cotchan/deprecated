---
title: JPQL) JPQL 기본 함수
author: cotchan 
date: 2021-01-31 04:39:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. JPQL 기본 함수

+ 사용할 수 있는 건 크게 3가지

+ JPQL 표준 함수 (데이터베이스에 관계 없이 사용하면 됩니다.)
  + CONCAT
    + 문자열 두 개 더하기
    + concat('a', 'b')
  + SUBSTRING
    + substring(m.username, 2, 3) //두 번째부터 3개 잘라내기
  + TRIM
    + 공백 제거
  + LOWER, UPPER
  + LENGTH
  + LOCATE
    + locate('de', 'abcdefg')
    + 결과: 숫자 4를 돌려줍니다.
  + ABS, SQRT, MOD
    + 수학 관련 함수
  + SIZE, INDEX(JPA 용도 - INDEX는 안 쓰는 게 낫습니다.)
    + 컬렉션 크기를 돌려줍니다.
    + String query = "select size(t.members) From Team t";

---

## 2. 사용자 정의 함수

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-basic-function-01.png)

 







---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
