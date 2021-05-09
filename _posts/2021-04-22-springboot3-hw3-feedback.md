---
title: sb3) 세 번째 과제 피드백(잘 몰랐던 상식 ver.3)
author: cotchan 
date: 2021-04-22 19:26:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Java 코딩 스타일 지키기

+ **아래 사항은 자바에서 잘 사용하지 않는 코딩 스타일입니다. 자바 표준 스타일에 맞춰보세요.**
  + **언더스코어 `_` 변수명**
  + **if 구문에서 줄바꿈 이후 대괄호(`{`) 사용**

---

## 2. DataAccessException

+ **`DataAccessException` 예외를 명시적으로 throws에 추가할 필요는 없습니다.**
+ 그렇게 사용한다면 모든 service 메소드에 추가해야하는 번거로움이 발생합니다.

---

## 3. 쿼리 사용이 지나치게 복잡했네요

+ 기본적인 SQL 및 join과 subquery에 대해 학습해보면 좋을것 같습니다.
+ **`Inner join`, `Left outer join` 적어도 이 두 개는 잘 숙지해놓으세요.**

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [DataAccessException이란? (What is DataAccessException?)](https://withseungryu.tistory.com/95)
